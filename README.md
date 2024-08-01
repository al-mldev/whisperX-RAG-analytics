

#### WhisperX Metrics

 Metrics project is designed to evaluate the performance of the speech to text process using a pretrained whisper faster model with [WhisperX](https://github.com/m-bain/whisperX) pipeline developed by [@m-bain](https://github.com/m-bain/) to generate the transcription and diarization of the audio conversation. The objective is to provide a framework for process the audio files as a batch load, generate the transcription, retrieve the diarization objects, and store them in a non-rel database, then evaluate the performance metrics of the transcription process within the loaded model. The module metrics can calculate the real time factor (RTF) for the audio file and if reference data is available the word error rate (WER) and character error rate (CER) can be calculated and stored into the database. The utils module allows to load whisper models that can be downloaded from huggingface, or manually copy into `models/` directory, and then transform it into whisper faster format. It can run transcriptions with mutiple models to the same audio and evaluate their individual performance based on the metrics. This speech to text process can be implemented into a ETL cycle for a large audio batch, generate new transcriptions, correct them and use as tagged audios for feed the training datasets for your models. 
 

#### Architecture 

![Pipeline](images/Architecture.png)

#### :white_check_mark: Instructions

####  ◽️ Previous settings:

##### 1) Set local environment

This version runs in a standalone windows local machine, but could run into a virtual environment.The needed dependencies are in requirements.txt, this specific code runs with whisperx 3.1.2, python 3.8. and CUDA 11.8. and the latest version of ffmpeg. 

`pip install -r requirements.txt`

##### 2) Set mongo db
 
Install mongodb and mongo compass, create the database `'whisperx'` then set the localhost uri, make sure to match with the one set in the connections of the project. 

##### 3) Set huggingface credentials  

Set `hf_token` with your huggingface token, and `model_name` the huggingface uri of yout desired pretrained model available in their site, the current model set for download is whisper tiny english once selectedz both run main.py


#### ◽️ Menu

Once you have your local environment, the connection to mongodb, and the hf credentials run main.py and the menu will display 

##### 1) Create Database Collections 

When the database connection is ready, select option 1 to insert the collections schema into the database. 

(output message image)

##### 2) Load Models

 Select the option 1 in menu to download the whisper model, and then convert to whisper faster format. You can also load manually in models directory. 
(ouput message image)

##### 3) Load Audios 

Copy your audios in `local_input_batch/audio_batch/`, make sure to use mp3 format. Then select option 3 in menu, you should see the new audio files in the database, in the `audio_files` collection. 

(audio file object image)


##### 4) Load References 

This is an optional step, you have manually corrected transcriptions, for the audio, copy your referene files into `local_input_batch/reference_batch/`, make sure to name `R-<audioname>.txt`. Select the option 3 in menu, provide the `audio_id` generated in the database for the asociated mp3 file previously loaded, then you should see the reference in `references` collection.

(reference object image)


##### 5) Run Transcriptions

Once all setup, select option 5 in menu then run the transcription. If references are not available, word and character error rate won't be calculated. 

Note: The ouput_text returns the not aligned text used as a reference format, you can check an example on `local_input_batch/reference_batch/R-audioexample.txt`, so if the model generate a 'visually consistent' transcription this new text can be used as a base to make a manual reference then calculate the wer and cer to the transcription asociated to the specific downloaded model. 

(transcription output image)

(metrics output image)

(transcription object image)

(metrics object image)


