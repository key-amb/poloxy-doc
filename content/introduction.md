+++
categories = ["general"]
date = "2016-05-14T15:38:27+09:00"
weight = 190
tags = ["document"]
title = "Introduction"

+++

## Concept of "poloxy"

The image bellow illustrates the concept of **"poloxy"**.

1. **API** receives alerts from monitoring systems and enqueues them.
1. **Worker** summarizes alerts.
1. Worker delivers them to recipients.

It prevents bursting alerts flood to recipients' phones or any devices.

<div align="center" style="border: 2px solid #aaa; padding: 10px;">
<img src="https://raw.githubusercontent.com/key-amb/poloxy/resource/image/concept.png" alt="poloxy-concept">
</div>

## Components

- **API**
  - Receives alerts from monitoring systems and enqueues them
- **Worker**
  - Dequeues alerts, summarizes them and delivers them to recipients
- **Web Dashboard**
  - Viewer for incoming alerts and delivered notifications
  - One can check original alerts on this dashboard
  - Integrated with **API**
- **CLI**
  - Utility tool for manual tasks such as data purge

## Public Resources

### Presentations

#### 2016 Apr. 28th - "Introduction to poloxy"

In [Nishi-Nippori.rb](https://nishinipporirb.doorkeeper.jp/).

<div align="center">
<a href="http://www.slideshare.net/YasutakeKiyoshi/introduction-to-poloxy-proxy-for-alerting" target="_blank">
<img src="https://raw.githubusercontent.com/key-amb/poloxy/resource/image/ninirb_20160428-screenshot.png" alt="ninirb_20160428-screenshot">
</a>
</div>

