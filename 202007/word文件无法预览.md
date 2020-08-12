# word文件无法预览

## 资源管理器预览word文件
修改注册表HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\PreviewHandlers\

把数据为“Microsoft Word Previewer”的名称记住，如果数值为“Microsoft Word 预览器”则修改为“Microsoft Word Previewer”。

然后找到HKEY_CLASSES_ROOT\.doc\ShellEx\{8895b1c6-b41f-4c1c-a562-0d564250836f}和HKEY_CLASSES_ROOT\.docx\ShellEx\{8895b1c6-b41f-4c1c-a562-0d564250836f}，将其中默认项的数据修改为之前记下的名称。

Excel文件和PPT文件同理。excel是找到.xlsx ppt是找到.pptx即可

转载 https://blog.csdn.net/luo_lucky/article/details/105970540