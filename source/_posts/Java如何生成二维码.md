---
title: Java如何生成二维码
date: 2022-04-25 21:44:40
tags: "Java"
---

Java生成二维码只需三步！！！
<!--more-->

## 一、导入Maven依赖

```
        <dependency>
            <groupId>com.google.zxing</groupId>
            <artifactId>core</artifactId>
            <version>3.3.0</version>
        </dependency>
        <dependency>
            <groupId>com.google.zxing</groupId>
            <artifactId>javase</artifactId>
            <version>3.3.0</version>
        </dependency>

```

## 二、核心工具类代码
```
public class QRCodeUtil {

    private static final String CHARSET = "utf-8";

    // 二维码尺寸
    private static final int QRCODE_SIZE = 400;

    // LOGO宽度
    private static final int LOGO_WIDTH = 120;
    // LOGO高度
    private static final int LOGO_HEIGHT = 120;

    public static BufferedImage createImage(String content) {
        Hashtable<EncodeHintType, Object> hints = new Hashtable<EncodeHintType, Object>();
        hints.put(EncodeHintType.ERROR_CORRECTION, ErrorCorrectionLevel.H);
        hints.put(EncodeHintType.CHARACTER_SET, CHARSET);
        hints.put(EncodeHintType.MARGIN, 1);
        BitMatrix bitMatrix = null;
        try {
            bitMatrix = new MultiFormatWriter().encode(content, BarcodeFormat.QR_CODE, QRCODE_SIZE, QRCODE_SIZE,
                    hints);
        } catch (WriterException e) {
            e.printStackTrace();
        }
        int width = bitMatrix.getWidth();
        int height = bitMatrix.getHeight();
        BufferedImage image = new BufferedImage(width, height, BufferedImage.TYPE_INT_RGB);
        for (int x = 0; x < width; x++) {
            for (int y = 0; y < height; y++) {
                image.setRGB(x, y, bitMatrix.get(x, y) ? 0xFF000000 : 0xFFFFFFFF);
            }
        }
        return image;
    }

    /**
     * 插入LOGO
     *
     * @param source       二维码图片
     * @param logoPath     LOGO图片地址
     * @param needCompress 是否压缩
     * @throws Exception
     */
    public static void insertImage(BufferedImage source, InputStream logoPath, boolean needCompress)  {
        Image src = null;
        try {
            src = ImageIO.read(logoPath);
        } catch (IOException e) {
            e.printStackTrace();
        }
        int width = src.getWidth(null);
        int height = src.getHeight(null);
        if (needCompress) {
            // 压缩LOGO
            if (width > LOGO_WIDTH) {
                width = LOGO_WIDTH;
            }
            if (height > LOGO_HEIGHT) {
                height = LOGO_HEIGHT;
            }
            Image image = src.getScaledInstance(width, height, Image.SCALE_SMOOTH);
            BufferedImage tag = new BufferedImage(width, height, BufferedImage.TYPE_INT_RGB);
            Graphics g = tag.getGraphics();
            // 绘制缩小后的图
            g.drawImage(image, 0, 0, null);
            g.dispose();
            src = image;
        }
        // 插入LOGO
        Graphics2D graph = source.createGraphics();
        int x = (QRCODE_SIZE - width) / 2;
        int y = (QRCODE_SIZE - height) / 2;
        graph.drawImage(src, x, y, width, height, null);
        Shape shape = new RoundRectangle2D.Float(x, y, width, width, 12, 12);
        graph.setStroke(new BasicStroke(3f));
        graph.draw(shape);
        graph.dispose();
    }
}

```

## 三、通过浏览器请求展示二维码
核心代码如下:
```
@GetMapping("/generateCode")
@ApiOperation("二维码生成")
public void generateCode(HttpServletResponse response) {
    BufferedImage image;
    // 禁止图像缓存
    response.setHeader("Pragma", "No-cache");
    response.setHeader("Cache-Control", "no-cache");
    //response.setHeader("content-disposition", "attachment;fileName=" + URLDecoder.decode("App二维码登录", "UTF-8"));
    response.setHeader("Content-Disposition", "inline");
    response.setDateHeader("Expires", 0);
    response.setContentType("image/jpeg");
    // 创建二进制的输出流
    try {
        String url = "a63GgF4C6yf1FE9O3aN9T4wzRLF5";
        image = QRCodeUtil.createImage(url);
        File f = new File("D://test//mylogo.jpg");
        InputStream inputStream = new FileInputStream(f);
        QRCodeUtil.insertImage(image, inputStream, Boolean.TRUE);
        ServletOutputStream sos = response.getOutputStream();
        // 将图像输出到Servlet输出流中。
        ImageIO.write(image, "jpeg", sos);
    } catch (IOException e) {
        e.printStackTrace();
    }
}

```