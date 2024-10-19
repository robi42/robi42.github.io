+++
title = "Pragmatic App Stats With R"
date = 2012-10-21
path = "blog/2012/10/21/pragmatic-app-stats-with-r"
[taxonomies]
#tags = ["Datavis", "Howto"]
+++

Here're some tips for conveniently generating automated visual app stats from MySQL data.

Recommended toolchain:

 * [R](https://www.r-project.org/) + [RStudio](https://posit.co/products/open-source/rstudio/)
 * [RMySQL](https://cran.r-project.org/web/packages/RMySQL/) + [ggplot2](https://ggplot2.tidyverse.org/)
 * *nix, cron + mailx & co.

Corresponding code bits to get one started:

## Simplified R script
```r
#!/usr/bin/Rscript

#install.packages('RMySQL')
#install.packages('ggplot2')

library(RMySQL)
library(ggplot2)
library(grid)


# PNG layout of plots helper.
vplayout <- function(x, y) viewport(layout.pos.row=x, layout.pos.col=y)


# Connect to DB.
dbUsername <- 'some_user'
dbPassword <- 'some_password'
dbName     <- 'app_stats_snapshot'
dbHost     <- 'localhost'

dbConnection <- dbConnect(MySQL(), user=dbUsername, password=dbPassword,
                          dbname=dbName, host=dbHost)


# Exemplary queries.
recordsPerDayQuery <-
  'select date(created_at) as Day, count(*) as Count from records where created_at >= "2012-1-1" and date(created_at) < date(now()) group by Day'

otherRecordsPerDayQuery <-
  'select date(created_at) as Day, count(*) as Count from other_records where created_at >= "2012-1-1" and date(created_at) < date(now()) group by Day'
# ...


# Query data and plot charts.
d1 <- dbGetQuery(dbConnection, recordsPerDayQuery)

xlabText <- '2012'; ylabText <- 'Count'
Records <- d1$Count
heading1 <- 'Records per Day'

plot1 <- qplot(as.Date(d1$Day), d1$Count, geom='line', color=Records,
               main=heading1, xlab=xlabText, ylab=ylabText)

d2 <- dbGetQuery(dbConnection, otherRecordsPerDayQuery)

Other <- d2$Count
heading2 <- 'Other Records per Day'

plot2 <- qplot(as.Date(d2$Day), d2$Count, geom='line', color=Other,
               main=heading2, xlab='2012', ylab='Count')
# ...


# Write data tables to mail text file.
mailTextFilename <- '/path/to/stats-mail.txt'

cat(paste('Hi,\n\n#', heading1, '\n'), file=mailTextFilename)
write.table(d1, file=mailTextFilename, append=TRUE,
            row.names=FALSE, quote=FALSE, sep=' | ')

cat(paste('\n#', heading2, '\n'), file=mailTextFilename, append=TRUE)
write.table(d2, file=mailTextFilename, append=TRUE,
            row.names=FALSE, quote=FALSE, sep=' | ')
# ...


# Make PNG with plotted charts.
png('/path/to/stats.png', width=1024, height=768)

grid.newpage()
pushViewport(viewport(layout=grid.layout(2, 1)))

print(plot1, vp=vplayout(1, 1))
print(plot2, vp=vplayout(2, 1))
# ...

dev.off()
```

## Related crontab sending stats mail once per week
```bash
# m h  dom mon dow   command
30 4 * * 1 /path/to/stats.r
0  5 * * 1 mailx -s '[Example] Stats' -a /path/to/stats.png stats@example.com < /path/to/stats-mail.txt
```

HTH.

