# Wav2Lip
<i>Creating lip-sync video using Wav2Lip model</i>
<li>Create lip-sync any video to any audio</li>

# Step by step process to create lip sync video using pre-trained Wav2Lip model
<b><h2>1. Set-up Wav2Lip</h2></b>
<br>!rm -rf /content/sample_data<br>
!mkdir /content/sample_data<br>
<br>
These commands are used for file and directory management in the context of a google colab notebook or a similar environment. In this specific case, you're deleting an existing directory (if it exists) and then creating a new one named sample_data. This could be useful if you want to organize and work with specific data in a clean environment.<br><br>
Make sure to change the runtime type to GPU
<br>
<br>
<b><li>To clone the model use the following command in google colab:</li></b>
<br>
!git clone https://github.com/zabique/Wav2Lip

It will create the Wav2Lip folder in your drive.<br>
The folder contains all required files for creating your lip-sync video.
<br><br>
<b><li>Download the pretrained model</li></b>
<br>
!wget 'https://iiitaphyd-my.sharepoint.com/personal/radrabha_m_research_iiit_ac_in/_layouts/15/download.aspx?share=EdjI7bZlgApMqsVoEUUXpLsBxqXbn5z8VTmoxp55YNDcIA' -O '/content/Wav2Lip/checkpoints/wav2lip_gan.pth'
<br>
a = !pip install https://raw.githubusercontent.com/AwaleSajil/ghc/master/ghc-1.0-py3-none-any.whl
<br><br>
<b><li>Install required libraries uing following command: </li></b>
<br>
!cd Wav2Lip && pip install -r requirements.txt
<br><br>
<b><li>Download pretrained model for face detection</li></b>
<br>
!wget "https://www.adrianbulat.com/downloads/python-fan/s3fd-619a316812.pth" -O "/content/Wav2Lip/face_detection/detection/sfd/s3fd.pth"
<br><br>
<b><li>Also install following:</li></b>
<br>
!pip install -q youtube-dl<br>
!pip install ffmpeg-python<br>
!pip install librosa==0.9.1
<br><br>
<b><li>Import the following:</li></b>
<br>
from IPython.display import HTML, Audio<br>
from google.colab.output import eval_js<br>
from base64 import b64decode<br>
import numpy as np<br>
from scipy.io.wavfile import read as wav_read<br>
import io<br>
import ffmpeg<br>
<br>
from IPython.display import clear_output <br>
clear_output()<br>
print("\nDone")<br><br>
<b><h2>2. Upload video file and corresponding audio file that we have to convert</h2></b>
<br>
%cd sample_data/<br>
from google.colab import files<br>
uploaded = files.upload()<br>
%cd ..<br>
<br>
<li>The audio file should be in .wav and video file in .mp4 extensions.<br>
<li>Make sure you use correct file extensions and both files have to be exact same length.<br>
<li>Target face in the input_video.mp4, must be "detectable" in ALL videoframes (So no black or blurry frames etc).<br>
<li>wav2lip does not like very long and high res clips (1080p/30seconds max).
<br><br>
<b><h2>3. Create Wav2Lip video (using wav2lip_gan.pth) GAN</h2></b>
<br>
!cd Wav2Lip && python inference.py --checkpoint_path checkpoints/wav2lip_gan.pth --face "/content/sample_data/input_video.mp4" --audio "/content/sample_data/input_audio.wav "
<br><br>
<li>Change the video file name and audio file name as yours.
<br><br>
<li><b>Use resize_factor to reduce the video resolution, as there is a change you might get better results for lower resolution videos. Why? Because the model was trained on low resolution faces. Here we used resize_factor=2. Change padding values to adjust the lip-sync of your video. Here we used 4 1 4 1(top bottom left right). Also you can change the fps(frames per second) to 30.</b>
<br><br>
  !cd Wav2Lip && python inference.py --checkpoint_path checkpoints/wav2lip_gan.pth --face "/content/sample_data/input_video.mp4" --audio "/content/sample_data/input_audio.wav" --resize_factor 2 --pads 4 1 4 1 --fps 30
<br><br>
<b><h2>4. Play result video - 50% scaling</h2></b>
<br>
from IPython.display import HTML<br>
from base64 import b64encode<br>
mp4 = open('/content/Wav2Lip/results/result_voice.mp4','rb').read()<br>
data_url = "data:video/mp4;base64," + b64encode(mp4).decode()<br>
HTML(f"""<br>
&lt;video width="50%" height="50%" controls><br>
  &lt;source src="{data_url}" type="video/mp4"><br>
&lt;/video>""")
<br><br>
<b><h2>5. Download Resultant video to your computer</h2></b>
<br><
from google.colab import files<br>
files.download('/content/Wav2Lip/results/result_voice.mp4') 
