from moviepy.editor import *
import os

# Download royalty-free assets
os.system('wget -O rain_bg.mp4 "https://cdn.pixabay.com/video/2020/09/04/50684-454857159_large.mp4"')
os.system('wget -O forest_bg.mp4 "https://cdn.pixabay.com/video/2021/08/12/85642-586173536_large.mp4"')
os.system('wget -O rain_audio.mp3 "https://cdn.pixabay.com/download/audio/2022/03/15/audio_87b9b1cf3c.mp3?filename=rain-and-thunder-ambient-110199.mp3"')

# Load media
rain_bg = VideoFileClip("rain_bg.mp4").loop(duration=1500).resize((1280,720))  # 25 min
forest_bg = VideoFileClip("forest_bg.mp4").loop(duration=300).resize((1280,720))  # 5 min
audio = AudioFileClip("rain_audio.mp3").subclip(0, 1800)  # 30 min total

# Define Pomodoro (25 min focus + 5 min break)
cycles = [("Focus Time", 25, rain_bg),
          ("Break Time", 5, forest_bg)]

clips = []
start_time = 0

for label, mins, bg in cycles:
    sec = mins * 60
    segment = bg.subclip(0, sec).set_start(start_time)

    txt_clips = []
    for t in range(sec):
        mm, ss = divmod(sec - t, 60)
        txt = f"{label}\n{mm:02d}:{ss:02d}"
        text = (TextClip(txt, fontsize=70, color="white", font="Arial-Bold")
                .set_position("center")
                .set_start(start_time + t)
                .set_duration(1))
        txt_clips.append(text)

    clips.append(segment)
    clips.extend(txt_clips)
    start_time += sec

final = CompositeVideoClip(clips, size=(1280, 720)).set_audio(audio)

# Export video
final.write_videofile("pomodoro_timer.mp4", fps=24, codec="libx264", audio_codec="aac")
