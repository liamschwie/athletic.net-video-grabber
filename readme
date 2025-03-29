# 🎥 Brightcove Video Download Guide  

A step-by-step tutorial to extract and download high-quality videos from Brightcove players.  

## 🔍 Overview  

This guide explains how to:  

✅ Find the hidden video stream URL from a Brightcove player page  
✅ Download the highest quality (1080p/4K) using `ffmpeg`  
✅ Bypass common restrictions (Referer/DRM)  

### 🎯 Works for:  

- Brightcove-hosted videos (e.g., news sites, educational platforms)  
- HLS (`.m3u8`) or MP4 streams  

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

## 🔧 Step 1: Locate the Video Stream URL  

### **Method A: Browser Dev Tools**  

1. Open the webpage with the Brightcove video.  
2. Press `F12` → Go to the **Network** tab.  
3. Filter for:  
   - `.m3u8` (HLS streams)  
   - `manifest.mpd` (DASH streams)  
   - `video.mp4` (direct MP4 files)  
4. Look for URLs like:  

```txt
https://cf-images.us-east-1.prod.boltdns.net/v1/playback/[ACCOUNT_ID]/[VIDEO_ID]/master.m3u8
```

### **Method B: Player Page Hack**  

Append `&isUI=false&isJSON=true` to the player URL to force a JSON response:  

```txt
https://players.brightcove.net/6055873638001/default_default/index.html?videoId=6370761103112&isUI=false&isJSON=true
```

➡ Extract the `sources[].src` URL from the JSON response.  

---

## 📥 Step 2: Download the Video  

### **Using ffmpeg (Best Quality)**  

```sh
ffmpeg -i "https://cf-images.../master.m3u8" \
  -c copy \               # Preserve original quality  
  -bsf:a aac_adtstoasc \  # Fix audio sync  
  "output_1080p.mp4"
```

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

---

This version improves readability, formatting, and structure while keeping everything clean and easy to follow. 🚀 Let me know if you’d like any further refinements! 😃
