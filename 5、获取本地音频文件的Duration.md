### 获取本地音频文件的Duration

    // get the duration of the audio.
    AudioFileID fileID;
    Float64 outDataSize = 0;
    UInt32 thePropSize = sizeof(Float64);
    OSStatus result = AudioFileOpenURL((__bridge CFURLRef)audioURL, kAudioFileReadPermission, 0, &fileID);
    result = AudioFileGetProperty(fileID, kAudioFilePropertyEstimatedDuration, &thePropSize, &outDataSize);
    AudioFileClose(fileID);
    
    NSLog(@"%f", outDataSize);

