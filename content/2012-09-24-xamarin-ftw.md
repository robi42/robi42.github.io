+++
title = "Xamarin FTW?"
date = 2012-09-24
path = "blog/2012/09/24/xamarin-ftw"
[taxonomies]
#tags = ["Code", "Mobile"]
+++

Often, when developing a mobile app one wants to target iOS as well as Android (+ maybe MS' OS).

Now, this basically means developing the app from scratch and consequently maintaining it at least twice on different platforms, written in different languages (Objective-C and Java, that is), using different sets of tools etc.

Cumbersome and potentially error-prone. Well, there's a viable alternative:

With [Xamarin](http://xamarin.com/) one can write mobile apps targetting iOS and Android in C# using .NET (and native) libraries, sharing code (business logic, data & web service layers, utilities, ...) while creating fully native UIs built on each platform's own SDKs, providing access to all respective device capabilities.

In addition to that, there's for example [MonoTouch.Dialog](http://blog.xamarin.com/2012/02/10/easily-create-ios-user-interfaces-with-monotouch-dialog/) making it easier and more fun to create table-based iOS UIs and for Android there's a useful visual [UI design tool](http://docs.xamarin.com/android/tutorials/Designer_Walkthrough) within the MonoDevelop IDE. Plus, there's [Xamarin.Mobile](http://xamarin.com/mobileapi) aimed at exposing an unified API facade for accessing common device features. All interesting stuff when used sensibly.

Here's some demo code (which is loosely based on samples from the recommendable book ["Mobile Development with C#"](http://shop.oreilly.com/product/0636920024002.do)) showing how mentioned shared layers could benefit:

## Simple model
```c#
public class Tweet
{
    public long     Id        { get; set; }
    public DateTime CreatedAt { get; set; }
    public string   Text      { get; set; }
}
```
## Simple REST API client
```c#
public class TwitterApiClient
{
    const string BaseUrl = "https://api.twitter.com/1/statuses/";

    public void DoWithTweetsForUser(string username, Action<IList<Tweet>> callback)
    {
        var webClient = new WebClient();
        var url = string.Format("{0}user_timeline.json?screen_name={1}",
                                BaseUrl, Uri.EscapeUriString(username));

        webClient.DownloadStringCompleted += (sender, e) => {
            var tweets = (from element in JsonValue.Parse(e.Result) as JsonArray
                let tweetData = element as JsonObject
                select new Tweet {
                    Id        = tweetData["id"],
                    CreatedAt = DateTime.ParseExact(tweetData["created_at"],
                                    "ddd MMM dd HH:mm:ss zz00 yyyy", null),
                    Text      = tweetData["text"],
                })
                .ToList();

            callback(tweets);
        };

        webClient.DownloadStringAsync(new Uri(url));
    }
}
```
## Exemplary user code
```c#
var apiClient = new TwitterApiClient();

apiClient.DoWithTweetsForUser("robi42", tweets => {
    foreach (var tweet in tweets) // Just for demo purpose.
        Debug.WriteLine("Tweet from {0}: {1}", tweet.CreatedAt, tweet.Text);
});
```

And with .NET 4.5's new [async features](http://blogs.msdn.com/b/dotnet/archive/2012/04/03/async-in-4-5-worth-the-await.aspx) landing in Monoland soon, related code will get even more convenient to write and handle.

Pretty neat, IMHO. What do you think?
