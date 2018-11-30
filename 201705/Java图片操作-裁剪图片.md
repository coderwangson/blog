 - 先给出裁剪一个图片的步骤 
 - // 首先通过ImageIo中的方法，创建一个Image + InputStream到内存
 - // 再按照指定格式构造一个Reader（Reader不能new的）
 - 	// 再通过ImageReader绑定 InputStream
 - // 设置感兴趣的源区域。
 - 	// 从 reader得到BufferImage
 - // 将BuffeerImage写出通过ImageIO

### 下面给出代码

```
	public static void cutImage(String filePath, int x, int y, int w, int h)
			throws Exception {
		// 首先通过ImageIo中的方法，创建一个Image + InputStream到内存
		ImageInputStream iis = ImageIO
				.createImageInputStream(new FileInputStream(filePath));
		// 再按照指定格式构造一个Reader（Reader不能new的）
		Iterator it = ImageIO.getImageReadersByFormatName("png");
		ImageReader imagereader = (ImageReader) it.next();
		// 再通过ImageReader绑定 InputStream
		imagereader.setInput(iis);

		// 设置感兴趣的源区域。
		ImageReadParam par = imagereader.getDefaultReadParam();
		par.setSourceRegion(new Rectangle(x, y, w, h));
		// 从 reader得到BufferImage
		BufferedImage bi = imagereader.read(0, par);

		// 将BuffeerImage写出通过ImageIO

		ImageIO.write(bi, "png", new File(filePath));

	}
```