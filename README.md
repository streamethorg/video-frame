# Video Frame Specification

This document provides a specification for enabling video playback using Farcaster Frames.

## Constructing a Video Frame

Video frames extend the current frame specification by introducing two new metadata tags.

| Tag                   | Type   | Description                                    |
| --------------------- | ------ | ---------------------------------------------- |
| `fc:frame:video`      | URL    | A URL string pointing to an HLS playlist index |
| `fc:frame:video:type` | String | A valid MIME type supported by video frames    |

For this initial release, we recommend supporting only HTTP Live Streaming (HLS) video types, as it is the most widely adopted video transmission protocol for both VOD (Video On Demand) and live video streaming.

Video frames should comply with the existing frame specification and use `fc:frame:image` and `og:frame:image` as fallback mechanisms.

### Aspect Ratio Implementation

The aspect ratio implementation details are yet to be defined.

## Rendering a Video Frame

Clients should adhere to the existing frame rendering rules, incorporating the following conditions into the process to support video frames:

1. Applications must inspect the headers to determine if the URL corresponds to a frame.
2. If the frame includes valid video frame tags, the application should render a video frame.
3. If the video frame tags are invalid or missing, applications should fallback to using OpenGraph (OG) tags.
4. In the absence of OG tags, applications should display a placeholder error message.

## Enabling Video Playback

Although HLS is extensively used in the streaming industry, it is not natively supported by all browsers. Therefore, applications should employ HTML5 video player libraries to ensure HLS compatibility across devices.

Below is a list of libraries that facilitate HLS playback:

- Video.js (https://videojs.com/)
- Hls.js (https://github.com/video-dev/hls.js)
- Livepeer.js (https://docs.livepeer.org/kit/player/Root)

This specification aims to standardize the integration and rendering of video content within Farcaster Frames, ensuring a consistent and accessible experience across platforms.
