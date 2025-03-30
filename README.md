# 🎥 Brightcove Video Download Guide  

A step-by-step tutorial to extract and download high-quality videos from Athletic.net race results using Brightcove players.  

## 🔍 Overview  

This guide explains how to:  

✅ Find the hidden video id from a Athletic.net race results page  
✅ Download the highest quality (1080p/4K) using `ffmpeg`  
✅ Bypass common restrictions (Referer/DRM)  

### 🎯 Works for:  

- Any results website using Athletic.net to store videos

---

## 🛠 Prerequisites  

Before you start, make sure you have:  

- A **browser** (Chrome/Firefox) with **Dev Tools** (`F12`)  
- **ffmpeg** (for downloading)  

### 🔹 Install ffmpeg (macOS)  

```sh
brew install ffmpeg
```

---

## 🔧 Step 1: Locate the Data Video ID

1. Navigate to the event you ran in the results
2. Press `F12` or right click and click inspect → Go to the **elements** tab.  
3. Press `CMD/CTRL + F` and Search for:  
   - `data-video-id` to find the video id
   - `data-heat-number` to find your heat (video id will be shown nearby)
   - You are looking for an element that looks like this:

```html
<div class="video-thumb--carousel-cell" data-heat-number="1" data-video-id="6370761263112"><div class="video-thumb--carousel--image"><img src="https://cf-images.us-east-1.prod.boltdns.net/v1/jit/6055873638001/c3249f27-4e4b-49fb-84d0-94a5859e0dd1/main/160x90/11s509ms/match/image.jpg"></div><div class="video-thumb--carousel--title ng-star-inserted">Heat 1</div><!----><!----><div class="video-thumb--carousel--duration ng-star-inserted">0:23</div><!----></div>
```

4. Copy the video id (Make sure you are copying the video id for the correct heat/event)
5. Go tho this URL but replace "(videoid)" with your video id:

```txt
https://players.brightcove.net/(videoid)/default_default/index.html?videoId=6370761103112&**isUI=false&isVideojs=true&isJSON=true**
```
For example:
```txt
https://players.brightcove.net/6055873638001/default_default/index.html?videoId=6370760085112&**isUI=false&isVideojs=true&isJSON=true**
```

## 📥 Step 2: Download the Video  

### **Using ffmpeg (Best Quality)**  

1. Run this command using terminal (modify download path if not on macOS):
```sh
ffmpeg -i "https://cf-images.../master.m3u8" \
  -c copy \               # Preserve original quality  
  -bsf:a aac_adtstoasc \  # Fix audio sync  
  "$HOME/Downloads/output_1080p.mp4"
```
2. After it is finished downloading, it should appear in your Downloads folder.

### **If Blocked by Referer**  

Some streams require a **Referer header** to authenticate. Add the original website’s domain:  

```sh
ffmpeg -headers "Referer: https://original-site.com" \
  -i "STREAM_URL" \
  -c copy "output.mp4"
```

---

## ⚠️ Troubleshooting  

| **Error**          | **Solution** |  
|--------------------|-------------|  
| `403 Forbidden`   | Add `-headers "Referer: [ORIGIN_URL]"` |  
| No streams found  | Check for DRM (e.g., Widevine) – try `yt-dlp` |  
| Invalid data      | Ensure URL ends with `.m3u8`/`.mp4` (not `.html`) |  

---

## 📜 Legal Note  

🔹 **Only download videos you have permission to access.**  
🔹 Brightcove’s **Terms of Service (ToS)** may prohibit unauthorized downloads.  

---

## 💡 Pro Tips  

🚀 **For 4K streams**, look for `rendition_2160p.m3u8` in the manifest.  
🎯 **Clip a segment** using `ffmpeg`:  

```sh
ffmpeg -i "STREAM_URL" -ss 00:10 -t 60 -c copy "clip.mp4"
```

---

## 🎉 Done!  

You now have the highest-quality video saved locally.  

📝 **For educational purposes only. Use responsibly.**  

---

## 🏆 Credits  

- Reverse-engineered from **Brightcove’s CMS API**  
- Tested on **macOS/Linux** with `ffmpeg 7.x`  
