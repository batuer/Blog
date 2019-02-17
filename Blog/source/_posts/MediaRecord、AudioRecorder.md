##### AudioRecord（基于字节流录音）

- 录制最原始的PCM流数据（PCM编码压缩后可为AMR、MP3、AAC），需要用AudioTrack播放。
- 可以实现语音的实时处理，进行边录边播，对音频的实时处理。

##### MediaRecorder（基于文件录音）

- 已集成录音、编码、压缩等，支持少量的音频格式文件。
- 录制的音频是经过编码压缩后的（AMR、MP3、AAC），并存储为文件。
- 优点：封装度高，操作简单。
- 缺点：不能实时处理音频，输出音频格式少。

###### 代码

```java
  /**
     * MediaRecord
     */
    private void mediaRecord() {
        //实例化MediaRecorder对象
        mMediaRecorder = new MediaRecorder();
        //从麦克风采集声音数据
        mMediaRecorder.setAudioSource(MediaRecorder.AudioSource.MIC);
        //设置输出格式为MP4
        mMediaRecorder.setOutputFormat(MediaRecorder.OutputFormat.MPEG_4);
        //设置采样频率44100 频率越高,音质越好,文件越大
        mMediaRecorder.setAudioSamplingRate(44100);
        //设置声音数据编码格式,音频通用格式是AAC
        mMediaRecorder.setAudioEncoder(MediaRecorder.AudioEncoder.AAC);
        //设置编码频率
        mMediaRecorder.setAudioEncodingBitRate(96000);
        //设置输出文件
        //创建录音文件
        File file = new File(Environment.getExternalStorageDirectory()
                .getAbsolutePath()
                + "/recorderDemo/" + System.currentTimeMillis() + ".m4a");
        File parentFile = file.getParentFile();
        if (!parentFile.exists()) parentFile.mkdirs();

        //开始录音
        try {
            file.createNewFile();
            String path = file.getPath();
            Log.w("Fire", "MediaActivity:50行:" + path);
            mMediaRecorder.setOutputFile(path);
            mMediaRecorder.prepare();
            mMediaRecorder.start();
            mMediaStartTime = System.currentTimeMillis();
            ToastUtils.showShort("Media正在录音: ");
        } catch (IOException e) {
            e.printStackTrace();
            Log.e("Fire", "MediaActivity:109行:" + e.toString());
            mMediaRecorder = null;
        }
    }

    /**
     * MediaStop
     */
    private void mediaStop() {
        mMediaRecorder.stop();
        mMediaRecorder.release();
        mMediaRecorder = null;
    }
```



##### MediaPlayer和AudioTrack

###### MediaPlayer

- 可播放多种格式的声音文件（MP3、AAC、WAV、OGG、MIDI等等）。
- 会在framework创建对应的**音频解码器**。
- 会在framework创建AudioTrack，把解码后的PCM流数据传递给AudioTrack，AudioTrack再传递给AudioFlinger进行混音，然后传给硬件播放。

###### AudioTrack

- 只能播放已经解码的PCM流，文件只支持不需要解码的wav。
- 