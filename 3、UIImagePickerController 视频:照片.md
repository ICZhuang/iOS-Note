### UIImagePickerController 视频/照片

> 微信将视频与照片合在一起

```objective-c

UIImagePickerController *picker = [[UIImagePickerController alloc] init];
picker.sourceType = UIImagePickerControllerSourceTypeCamera; // 资源类型
picker.mediaTypes = [UIImagePickerController 				
                     availableMediaTypesForSourceType:picker.sourceType]; // 媒体类型
picker.videoQuality = UIImagePickerControllerQualityTypeLow;
[self.window.rootViewController presentViewController:picker animated:YES completion:nil];
```