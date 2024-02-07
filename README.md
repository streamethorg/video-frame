# Video Frame Specification

This document provides a specification for enabling video playback using Farcaster Frames.

## Constructing a Video Frame

Video frames extend the current [frame specification](https://github.com/farcasterxyz/docs/blob/d352d03b40c33092b6f659c2882308b90e6b6b1c/docs/reference/frames/spec.md) by introducing two new metadata tags.

| Tag                   | Type   | Description                                    |
| --------------------- | ------ | ---------------------------------------------- |
| `fc:frame:video`      | URL    | A URL string pointing to an HLS playlist index |
| `fc:frame:video:type` | String | A valid MIME type supported by video frames    |

For this initial release, we recommend supporting only HTTP Live Streaming (HLS) video types, as it is the most widely adopted video transmission protocol for both VOD (Video On Demand) and live video streaming.

Video frames should comply with the existing frame specification and use `fc:frame:image` and `og:frame:image` as fallback mechanisms.

### Aspect Ratio Implementation

We recommend for the maximum aspect ratio to be `1920 × 1080`.

## Rendering a Video Frame

Clients should adhere to the existing frame rendering rules, optionally incorporating the following conditions into the process if they want to support video frames:

1. Applications must inspect the headers to determine if the URL corresponds to a frame.
2. If the frame includes valid video frame tags, the application should render a video frame.
3. If the video frame tags are invalid or missing, applications should fallback to using OpenGraph (OG) tags.
4. In the absence of OG tags, applications should display a placeholder error message.
5. In the case where an OG image tag exists, the image URL should be treated as the cover image of the video.

## Enabling Video Playback

Although HLS is extensively used in the streaming industry, it is not natively supported by all browsers. Therefore, applications should employ HTML5 video player libraries to ensure HLS compatibility across devices.

Below is a list of libraries that facilitate HLS playback:

- [Video.js](https://videojs.com/)
- [Hls.js](https://github.com/video-dev/hls.js)
- [Livepeer.js](https://docs.livepeer.org/kit/player/Root)

In the case of mobile applications, iOS supports native HLS playback and Android supports [ExoPlayer](https://exoplayer.dev/). There are also other open source / commercial video players that can provide more consistent cross-platform support.  For exmaple, [livepeer.js](https://docs.livepeer.org/kit/player/Root) contains a react-native SDK,  

This specification aims to standardize the integration and rendering of video content within Farcaster Frames, ensuring a consistent and accessible experience across platforms.

## Examples

- [index.html](https://streamethorg.github.io/video-frame/frame.html) outlines how a video frame should be implemented.
- [frame.html](https://streamethorg.github.io/video-frame) outlines how a video frame should be rendered. Of course, this is a quick demo implementation, fetch requests should be proxied on a server.

## Performance Considerations

### Frame Developers
Since frame developers are ultimately responsible for the performance of a video frame, it is recommended to use a video streaming provider that offers good performance. Decentralized solutions like [Livepeer Studio](https://livepeer.studio/) can provide cost effectiveness and ease of use. More traditional cloud providers like AWS, Mux, and Cloudflare can be a good option as well. You can also consider building your own storage / transcoding / CDN delivery tech stack for more fine-grained control. It's important to keep track of video streaming performance metrics like error rate, rebuffer rate, and time-to-first-frame.

### App Developers
Video streaming is notoriously difficult due to the high requirement in bandwidth. Even though HLS helps by providing different renditions and doing [adaptive bitrate streaming](https://en.wikipedia.org/wiki/Adaptive_bitrate_streaming), a playlist is not guaranteed to contain the appropriate renditions.  Thus, it is highly recommended for client developers to pre-cache video content, at least in a feed.  Perceived user experience can be further enhanced by keeping track of error / buffering events, and removing video frames that show clear performance issues.
