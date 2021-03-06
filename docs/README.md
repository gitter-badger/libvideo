# Usage

libvideo has a very simple API. First, make sure you have the appropriate statement at the top of your file.

```csharp
using VideoLibrary;
```

Great! Now let's get started.

The entry point for most of the API is in `YouTubeService`:

```csharp
var service = new YouTubeService();
```

To download a video or get a URI for download, simply type this:

```csharp
byte[] bytes = service.Download(videoUri);
string downloadUri = service.GetUri(videoUri);
```

YouTube exposes multiple URIs for each video, which vary in quality and size. To download all of them:

```csharp
IEnumerable<byte[]> arrays = service.DownloadMany(videoUri);
IEnumerable<string> downloadUris = service.GetAllUris(videoUri);
```

## Advanced

libvideo has full support for async callers; if you need the asynchronous version of a method, just append `Async` to the name. For example:

```csharp
byte[] bytes = await service.DownloadAsync(videoUri);
string downloadUri = await service.GetUriAsync(videoUri);
```

If you need more information about the video, such as bitrate or resolution, you can use the following methods:

```csharp
Video video = service.GetVideo(videoUri);
IEnumerable<Video> videos = service.GetAllVideos(videoUri);
```

The `Video` class enscapulates more detailed information about the video, and includes a `GetBytes()` method for convenience.

If you already have an `HttpClient`, `WebClient`, or `HttpWebRequest` in use and you don't want libvideo to create a new one every time it visits YouTube, don't worry! Just pass in a delegate describing how to download the page:

```csharp
using (var client = new WebClient())
{
    // Do some work with WebClient...
    service.Download(videoUri, uri => client.DownloadString(uri));
}
```

This works for asynchronous clients as well:

```csharp
using (var client = new HttpClient())
{
    // Do some work with HttpClient...
    await service.DownloadAsync(videoUri, uri => client.GetStringAsync(uri));
}
```

If you want to download a video from another site, such as Vimeo, libvideo supports that as well:

```csharp
var service = new VimeoService();
```

Because `VimeoService` and `YouTubeService` share a common ancestor, `ServiceBase`, you can call the same methods as you would for YouTube's service.

---

That's it! We hope you enjoy libvideo. If you're looking for more advanced functionality, feel free to raise an issue and we'll look into it.
