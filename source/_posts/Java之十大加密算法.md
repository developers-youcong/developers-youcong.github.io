---
title: Java之十大加密算法
date: 2022-08-03 19:14:28
tags: "Java"
---

基于Java内部API编写而成的十大加密算法分享！！！
<!--more-->
YC-Framework已经将十大加密算法集成进去了，欢迎大家star或fork！！！

YC-Framework源代码地址如下（包含Github以及Gitee）：

https://github.com/developers-youcong/yc-framework

https://gitee.com/developers-youcong/yc-framework

## 一、AES
```
public class AESUtil {

    /**
     * AES解密
     *
     * @param encryptStr 密文
     * @param decryptKey 秘钥，必须为16个字符组成
     * @return 明文
     * @throws Exception
     */
    public static String aesDecrypt(String encryptStr, String decryptKey) throws Exception {
        if (StringUtils.isEmpty(encryptStr) || StringUtils.isEmpty(decryptKey)) {
            return null;
        }

        byte[] encryptByte = Base64.getDecoder().decode(encryptStr);
        Cipher cipher = Cipher.getInstance("AES/ECB/PKCS5Padding");
        cipher.init(Cipher.DECRYPT_MODE, new SecretKeySpec(decryptKey.getBytes(), "AES"));
        byte[] decryptBytes = cipher.doFinal(encryptByte);
        return new String(decryptBytes);
    }

    /**
     * AES加密
     *
     * @param content    明文
     * @param encryptKey 秘钥，必须为16个字符组成
     * @return 密文
     * @throws Exception
     */
    public static String aesEncrypt(String content, String encryptKey) throws Exception {
        if (StringUtils.isEmpty(content) || StringUtils.isEmpty(encryptKey)) {
            return null;
        }

        Cipher cipher = Cipher.getInstance("AES/ECB/PKCS5Padding");
        cipher.init(Cipher.ENCRYPT_MODE, new SecretKeySpec(encryptKey.getBytes(), "AES"));

        byte[] encryptStr = cipher.doFinal(content.getBytes(StandardCharsets.UTF_8));
        return Base64.getEncoder().encodeToString(encryptStr);
    }

    // 测试加密与解密
    public static void main(String[] args) {
        try {
            String secretKey = "676#@,!";
            String content = "123456";
            String s1 = aesEncrypt(content, secretKey);
            System.out.println(s1);
            String s = aesDecrypt(s1, secretKey);
            System.out.println(s);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }


}

```

## 二、Blowfish/ECB/NoPadding
```
public class BlowfishECBNoPaddingUtil {
    public static void main(String[] ags) {
        String key = "x.1234dsadftwer";
        String data = "Java Blowfish ECB NoPadding";
        System.out.println("原始字符串:" + data);
        //加密
        String encrypt = encodingToBase64(key, data);
        System.out.println("加密结果:" + encrypt);
        //解密
        String decodingByBase64 = decodingByBase64(key, encrypt);
        System.out.println("解密结果:" + decodingByBase64);
        System.out.println("是否完全相等:" + data.equals(decodingByBase64));
    }


    /**
     * Blowfish/ECB/NoPadding 加密并输出base64
     * 不足8倍数自动尾部补充0
     *
     * @param key  密钥key
     * @param data 要加密的字符串
     * @return 加密后的字符串
     */
    public static String encodingToBase64(String key, String data) {
        try {
            Cipher cipher = Cipher.getInstance("Blowfish/ECB/NoPadding");
            int blockSize = cipher.getBlockSize();
            byte[] dataBytes = data.getBytes();
            int length = dataBytes.length;
            //计算需填充长度
            if (length % blockSize != 0) {
                length = length + (blockSize - (length % blockSize));
            }
            byte[] plaintext = new byte[length];
            //填充
            System.arraycopy(dataBytes, 0, plaintext, 0, dataBytes.length);
            SecretKeySpec keySpec = new SecretKeySpec(key.getBytes(), "Blowfish");
            cipher.init(Cipher.ENCRYPT_MODE, keySpec);
            return java.util.Base64.getEncoder().encodeToString(cipher.doFinal(plaintext));

        } catch (Exception e) {
            e.printStackTrace();
            return null;
        }
    }

    /**
     * Blowfish/ECB/NoPadding 解密base64的字符串
     * 自动去除尾部补充的0
     *
     * @param key 密钥key
     * @param str 要解密的字符串
     * @return 解密后的字符串
     */
    public static String decodingByBase64(String key, String str) {
        SecretKeySpec skeySpec = new SecretKeySpec(key.getBytes(), "Blowfish");
        Cipher cipher;

        String cipherInstName = "Blowfish/ECB/NoPadding";
        byte[] doFinal;

        try {
            cipher = Cipher.getInstance(cipherInstName);
            cipher.init(Cipher.DECRYPT_MODE, skeySpec);
            doFinal = cipher.doFinal(java.util.Base64.getDecoder().decode(str.getBytes()));
        } catch (Exception e) {
            e.printStackTrace();
            return null;
        }

        int zeroIndex = doFinal.length;
        for (int i = doFinal.length - 1; i > 0; i--) {
            if (doFinal[i] == (byte) 0) {
                zeroIndex = i;
            } else {
                break;
            }
        }
        // 删除末尾填充的0
        doFinal = Arrays.copyOf(doFinal, zeroIndex);

        return new String(doFinal, StandardCharsets.UTF_8);
    }


}


```

## 三、DES
```
public class DESUtil {

    /**
     * 偏移变量，固定占8位字节
     */
    private final static String IV_PARAMETER = "76234134";
    /**
     * 密钥算法
     */
    private static final String ALGORITHM = "DES";
    /**
     * 加密/解密算法-工作模式-填充模式
     */
    private static final String CIPHER_ALGORITHM = "DES/CBC/PKCS5Padding";
    /**
     * 默认编码
     */
    private static final String CHARSET = "utf-8";


    /**
     * 生成key
     *
     * @param password
     * @return
     * @throws Exception
     */
    private static Key generateKey(String password) throws Exception {
        DESKeySpec dks = new DESKeySpec(password.getBytes(CHARSET));
        SecretKeyFactory keyFactory = SecretKeyFactory.getInstance(ALGORITHM);
        return keyFactory.generateSecret(dks);
    }


    /**
     * DES加密字符串
     *
     * @param password 加密密码，长度不能够小于8位
     * @param data     待加密字符串
     * @return 加密后内容
     */
    public static String encrypt(String password, String data) {
        if (password == null || password.length() < 8) {
            throw new RuntimeException("加密失败，key不能小于8位");
        }
        if (data == null)
            return null;
        try {
            Key secretKey = generateKey(password);
            Cipher cipher = Cipher.getInstance(CIPHER_ALGORITHM);
            IvParameterSpec iv = new IvParameterSpec(IV_PARAMETER.getBytes(CHARSET));
            cipher.init(Cipher.ENCRYPT_MODE, secretKey, iv);
            byte[] bytes = cipher.doFinal(data.getBytes(CHARSET));

            //JDK1.8及以上可直接使用Base64，JDK1.7及以下可以使用BASE64Encoder
            //Android平台可以使用android.util.Base64
            return new String(Base64.getEncoder().encode(bytes));

        } catch (Exception e) {
            e.printStackTrace();
            return data;
        }
    }

    /**
     * DES解密字符串
     *
     * @param password 解密密码，长度不能够小于8位
     * @param data     待解密字符串
     * @return 解密后内容
     */
    public static String decrypt(String password, String data) {
        if (password == null || password.length() < 8) {
            throw new RuntimeException("加密失败，key不能小于8位");
        }
        if (data == null)
            return null;
        try {
            Key secretKey = generateKey(password);
            Cipher cipher = Cipher.getInstance(CIPHER_ALGORITHM);
            IvParameterSpec iv = new IvParameterSpec(IV_PARAMETER.getBytes(CHARSET));
            cipher.init(Cipher.DECRYPT_MODE, secretKey, iv);
            return new String(cipher.doFinal(Base64.getDecoder().decode(data.getBytes(CHARSET))), CHARSET);
        } catch (Exception e) {
            e.printStackTrace();
            return data;
        }
    }

    /**
     * DES加密文件
     *
     * @param srcFile  待加密的文件
     * @param destFile 加密后存放的文件路径
     * @return 加密后的文件路径
     */
    public static String encryptFile(String password, String srcFile, String destFile) {

        if (password == null || password.length() < 8) {
            throw new RuntimeException("加密失败，key不能小于8位");
        }
        try {
            IvParameterSpec iv = new IvParameterSpec(IV_PARAMETER.getBytes(CHARSET));
            Cipher cipher = Cipher.getInstance(CIPHER_ALGORITHM);
            cipher.init(Cipher.ENCRYPT_MODE, generateKey(password), iv);
            InputStream is = new FileInputStream(srcFile);
            OutputStream out = new FileOutputStream(destFile);
            CipherInputStream cis = new CipherInputStream(is, cipher);
            byte[] buffer = new byte[1024];
            int r;
            while ((r = cis.read(buffer)) > 0) {
                out.write(buffer, 0, r);
            }
            cis.close();
            is.close();
            out.close();
            return destFile;
        } catch (Exception ex) {
            ex.printStackTrace();
        }
        return null;
    }

    /**
     * DES解密文件
     *
     * @param srcFile  已加密的文件
     * @param destFile 解密后存放的文件路径
     * @return 解密后的文件路径
     */
    public static String decryptFile(String password, String srcFile, String destFile) {
        if (password == null || password.length() < 8) {
            throw new RuntimeException("加密失败，key不能小于8位");
        }
        try {
            File file = new File(destFile);
            if (!file.exists()) {
                file.getParentFile().mkdirs();
                file.createNewFile();
            }
            IvParameterSpec iv = new IvParameterSpec(IV_PARAMETER.getBytes(CHARSET));
            Cipher cipher = Cipher.getInstance(CIPHER_ALGORITHM);
            cipher.init(Cipher.DECRYPT_MODE, generateKey(password), iv);
            InputStream is = new FileInputStream(srcFile);
            OutputStream out = new FileOutputStream(destFile);
            CipherOutputStream cos = new CipherOutputStream(out, cipher);
            byte[] buffer = new byte[1024];
            int r;
            while ((r = is.read(buffer)) >= 0) {
                cos.write(buffer, 0, r);
            }
            cos.close();
            is.close();
            out.close();
            return destFile;
        } catch (Exception ex) {
            ex.printStackTrace();
        }
        return null;
    }

    public static void main(String[] args) {
        String key = "^234!#sdf653214";
        String content = "Java DES";
        // 加密
        String s2 = encrypt(key, content);
        System.out.println("s2====" + s2);
        //解密
        String s3 = decrypt(key, s2);
        System.out.println("s3====" + s3);
    }
}

```

## 四、DH
```
public class DHUtil {
    /**
     * 定义加密方式
     */
    private static final String KEY_DH = "DH";
    public static final String PUBLIC_KEY = "DHPublicKey";
    public static final String PRIVATE_KEY = "DHPrivateKey";
    // 开始生成本地密钥SecretKey 密钥算法为对称密码算法
    // 可以为 DES DES AES
    public static final String KEY_DH_DES = "DES";

    /**
     * 甲方初始化并返回密钥对
     *
     * @return
     */
    public static Map<String, Object> initKey() {
        try {
            // 实例化密钥对生成器
            KeyPairGenerator keyPairGenerator = KeyPairGenerator
                    .getInstance(KEY_DH);
            // 初始化密钥对生成器 默认是1024 512-1024 & 64的倍数
            keyPairGenerator.initialize(1024);
            // 生成密钥对
            KeyPair keyPair = keyPairGenerator.generateKeyPair();
            // 得到甲方公钥
            DHPublicKey publicKey = (DHPublicKey) keyPair.getPublic();
            // 得到甲方私钥
            DHPrivateKey privateKey = (DHPrivateKey) keyPair.getPrivate();
            // 将公钥和私钥封装在Map中， 方便之后使用
            Map<String, Object> keyMap = new HashMap<String, Object>();
            keyMap.put(PUBLIC_KEY, publicKey);
            keyMap.put(PRIVATE_KEY, privateKey);
            return keyMap;
        } catch (Exception e) {
            throw new RuntimeException(e);
        }
    }

    /**
     * 乙方根据甲方公钥初始化并返回密钥对
     *
     * @param key 甲方的公钥
     * @return
     */
    public static Map<String, Object> initKey(byte[] key) {
        try {
            // 将甲方公钥从字节数组转换为PublicKey
            X509EncodedKeySpec keySpec = new X509EncodedKeySpec(key);
            // 实例化密钥工厂
            KeyFactory keyFactory = KeyFactory.getInstance(KEY_DH);
            // 产生甲方公钥pubKey
            DHPublicKey dhPublicKey = (DHPublicKey) keyFactory
                    .generatePublic(keySpec);
            // 剖析甲方公钥，得到其参数
            DHParameterSpec dhParameterSpec = dhPublicKey.getParams();
            // 实例化密钥对生成器
            KeyPairGenerator keyPairGenerator = KeyPairGenerator
                    .getInstance(KEY_DH);
            // 用甲方公钥初始化密钥对生成器
            keyPairGenerator.initialize(dhParameterSpec);
            // 产生密钥对
            KeyPair keyPair = keyPairGenerator.generateKeyPair();
            // 得到乙方公钥
            DHPublicKey publicKey = (DHPublicKey) keyPair.getPublic();
            // 得到乙方私钥
            DHPrivateKey privateKey = (DHPrivateKey) keyPair.getPrivate();
            // 将公钥和私钥封装在Map中， 方便之后使用
            Map<String, Object> keyMap = new HashMap<String, Object>();
            keyMap.put(PUBLIC_KEY, publicKey);
            keyMap.put(PRIVATE_KEY, privateKey);
            return keyMap;
        } catch (Exception e) {
            throw new RuntimeException(e);
        }
    }

    /**
     * 根据对方的公钥和自己的私钥生成 本地密钥,返回的是SecretKey对象的字节数组
     *
     * @param publicKey  公钥
     * @param privateKey 私钥
     * @return
     */
    public static byte[] getSecretKeyBytes(byte[] publicKey, byte[] privateKey) {
        try {
            // 实例化密钥工厂
            KeyFactory keyFactory = KeyFactory.getInstance(KEY_DH);
            // 将公钥从字节数组转换为PublicKey
            X509EncodedKeySpec pubKeySpec = new X509EncodedKeySpec(publicKey);
            PublicKey pubKey = keyFactory.generatePublic(pubKeySpec);
            // 将私钥从字节数组转换为PrivateKey
            PKCS8EncodedKeySpec priKeySpec = new PKCS8EncodedKeySpec(privateKey);
            PrivateKey priKey = keyFactory.generatePrivate(priKeySpec);

            // 准备根据以上公钥和私钥生成本地密钥SecretKey
            // 先实例化KeyAgreement
            KeyAgreement keyAgreement = KeyAgreement.getInstance(KEY_DH);
            // 用自己的私钥初始化keyAgreement
            keyAgreement.init(priKey);
            // 结合对方的公钥进行运算
            keyAgreement.doPhase(pubKey, true);
            // 开始生成本地密钥SecretKey 密钥算法为对称密码算法
            SecretKey secretKey = keyAgreement.generateSecret(KEY_DH_DES);
            return secretKey.getEncoded();
        } catch (Exception e) {
            throw new RuntimeException(e);
        }
    }

    /**
     * 根据对方的公钥和自己的私钥生成 本地密钥,返回的是SecretKey对象
     *
     * @param publicKey  公钥
     * @param privateKey 私钥
     * @return
     */
    public static SecretKey getSecretKey(byte[] publicKey, byte[] privateKey) {
        try {
            // 实例化密钥工厂
            KeyFactory keyFactory = KeyFactory.getInstance(KEY_DH);
            // 将公钥从字节数组转换为PublicKey
            X509EncodedKeySpec pubKeySpec = new X509EncodedKeySpec(publicKey);
            PublicKey pubKey = keyFactory.generatePublic(pubKeySpec);
            // 将私钥从字节数组转换为PrivateKey
            PKCS8EncodedKeySpec priKeySpec = new PKCS8EncodedKeySpec(privateKey);
            PrivateKey priKey = keyFactory.generatePrivate(priKeySpec);

            // 准备根据以上公钥和私钥生成本地密钥SecretKey
            // 先实例化KeyAgreement
            KeyAgreement keyAgreement = KeyAgreement.getInstance(KEY_DH);
            // 用自己的私钥初始化keyAgreement
            keyAgreement.init(priKey);
            // 结合对方的公钥进行运算
            keyAgreement.doPhase(pubKey, true);
            // 开始生成本地密钥SecretKey 密钥算法为对称密码算法
            SecretKey secretKey = keyAgreement.generateSecret(KEY_DH_DES);
            return secretKey;
        } catch (Exception e) {
            throw new RuntimeException(e);
        }
    }

    /**
     * 从 Map 中取得公钥
     *
     * @param keyMap
     * @return
     */
    public static byte[] getPublicKey(Map<String, Object> keyMap) {
        DHPublicKey key = (DHPublicKey) keyMap.get(PUBLIC_KEY);
        return key.getEncoded();
    }

    /**
     * 从 Map 中取得私钥
     *
     * @param keyMap
     * @return
     */
    public static byte[] getPrivateKey(Map<String, Object> keyMap) {
        DHPrivateKey key = (DHPrivateKey) keyMap.get(PRIVATE_KEY);
        return key.getEncoded();
    }

    /**
     * DH 加密
     *
     * @param data       带加密数据
     * @param publicKey  甲方公钥
     * @param privateKey 乙方私钥
     * @return
     */
    public static byte[] encryptDH(byte[] data, byte[] publicKey,
                                   byte[] privateKey) {
        byte[] bytes = null;
        try {
            //
            SecretKey secretKey = getSecretKey(publicKey, privateKey);
            // 数据加密
            Cipher cipher = Cipher.getInstance(secretKey.getAlgorithm());
            cipher.init(Cipher.ENCRYPT_MODE, secretKey);
            bytes = cipher.doFinal(data);
        } catch (NoSuchAlgorithmException e) {
            e.printStackTrace();
        } catch (NoSuchPaddingException e) {
            e.printStackTrace();
        } catch (InvalidKeyException e) {
            e.printStackTrace();
        } catch (BadPaddingException e) {
            e.printStackTrace();
        } catch (IllegalBlockSizeException e) {
            e.printStackTrace();
        }
        return bytes;
    }

    /**
     * DH 解密
     *
     * @param data       待解密数据
     * @param publicKey  乙方公钥
     * @param privateKey 甲方私钥
     * @return
     */
    public static byte[] decryptDH(byte[] data, byte[] publicKey,
                                   byte[] privateKey) {
        byte[] bytes = null;
        try {
            //
            SecretKey secretKey = getSecretKey(publicKey, privateKey);
            // 数据加密
            Cipher cipher = Cipher.getInstance(secretKey.getAlgorithm());
            cipher.init(Cipher.DECRYPT_MODE, secretKey);
            bytes = cipher.doFinal(data);
        } catch (NoSuchAlgorithmException e) {
            e.printStackTrace();
        } catch (NoSuchPaddingException e) {
            e.printStackTrace();
        } catch (InvalidKeyException e) {
            e.printStackTrace();
        } catch (BadPaddingException e) {
            e.printStackTrace();
        } catch (IllegalBlockSizeException e) {
            e.printStackTrace();
        }
        return bytes;
    }

    public static String fromBytesToHex(byte[] resultBytes) {
        StringBuilder builder = new StringBuilder();
        for (int i = 0; i < resultBytes.length; i++) {
            if (Integer.toHexString(0xFF & resultBytes[i]).length() == 1) {
                builder.append("0").append(
                        Integer.toHexString(0xFF & resultBytes[i]));
            } else {
                builder.append(Integer.toHexString(0xFF & resultBytes[i]));
            }
        }
        return builder.toString();
    }

    // 待加密的明文
    public static final String DATA = "Java DH";

    public static void main(String[] args) throws Exception {
        /* Test DH */
        // 甲方公钥
        byte[] publicKey1;
        // 甲方私钥
        byte[] privateKey1;
        // 甲方本地密钥
        byte[] secretKey1;
        // 乙方公钥
        byte[] publicKey2;
        // 乙方私钥
        byte[] privateKey2;
        // 乙方本地密钥
        byte[] secretKey2;

        // 初始化密钥 并生成甲方密钥对
        Map<String, Object> keyMap1 = DHUtil.initKey();
        publicKey1 = DHUtil.getPublicKey(keyMap1);
        privateKey1 = DHUtil.getPrivateKey(keyMap1);
        System.out
                .println("DH 甲方公钥 : " + fromBytesToHex(publicKey1));
        System.out.println("DH 甲方私钥 : "
                + fromBytesToHex(privateKey1));

        // 乙方根据甲方公钥产生乙方密钥对
        Map<String, Object> keyMap2 = DHUtil.initKey(publicKey1);
        publicKey2 = DHUtil.getPublicKey(keyMap2);
        privateKey2 = DHUtil.getPrivateKey(keyMap2);
        System.out
                .println("DH 乙方公钥 : " + fromBytesToHex(publicKey2));
        System.out.println("DH 乙方私钥 : "
                + fromBytesToHex(privateKey2));

        // 对于甲方， 根据其私钥和乙方发过来的公钥， 生成其本地密钥secretKey1
        secretKey1 = DHUtil.getSecretKeyBytes(publicKey2, privateKey1);
        System.out.println("DH 甲方 本地密钥 : "
                + fromBytesToHex(secretKey1));

        // 对于乙方， 根据其私钥和甲方发过来的公钥， 生成其本地密钥secretKey2
        secretKey2 = DHUtil.getSecretKeyBytes(publicKey1, privateKey2);
        System.out.println("DH 乙方 本地密钥 : "
                + fromBytesToHex(secretKey2));
        // ---------------------------
        // 测试数据加密和解密
        System.out.println("加密前的数据" + DATA);
        // 甲方进行数据的加密
        // 用的是甲方的私钥和乙方的公钥
        byte[] encryptDH = DHUtil.encryptDH(DATA.getBytes(), publicKey2,
                privateKey1);
        System.out.println("加密后的数据 字节数组转16进制显示"
                + fromBytesToHex(encryptDH));
        // 乙方进行数据的解密
        // 用的是乙方的私钥和甲方的公钥
        byte[] decryptDH = DHUtil.decryptDH(encryptDH, publicKey1, privateKey2);
        System.out.println("解密后数据:" + new String(decryptDH));
    }
}


```

## 五、ECDSA
```
public class ECDSAUtil {

    private static String data = "ECDSA Java";
//    private final static String publicKey1 = "3059301306072A8648CE3D020106082A8648CE3D03010703420004A416FD8C6572CCD2345AC3310A02D44A0BF2BC2E22153A26ABE0CB01EFC62D286D002D9B1328590EE2A8890D52E54652EA1AC54FB9699A98953128B20A177734";
//    private final static String privateKey1 = "3041020100301306072A8648CE3D020106082A8648CE3D030107042730250201010420D0C7C40D4841EFA1F2AB1831C18EA8CDF73974F26475E0DFA393D822FA475FF7";
//    private final static String publicKey2 = "3059301306072a8648ce3d020106082a8648ce3d03010703420004767c30eae6ef41a24de0b61b730f5990924c7427d2f215a3478a871c0de4dfe34b5e6c275d447098d864499491c669012e8c2c6aeb1c8cffff3e34a5f8515c9a";
//    private final static String privateKey2 = "3041020100301306072a8648ce3d020106082a8648ce3d03010704273025020101042011353b6b2ee5975cf2c7efb836329a07794be70b58d64efc23e7311fb4352501";

    public static void main(String[] args) throws Exception {
//        加签验签
        KeyPair keyPair = getKeyPair();
        PrivateKey privateKey = keyPair.getPrivate();
        PublicKey publicKey = keyPair.getPublic();
        String sign = signECDSA(privateKey, data);
        verifyECDSA(publicKey, sign, data);

//        生成公钥私钥1
//        KeyPair keyPair1 = getKeyPair();
//        PublicKey publicKey1 = keyPair1.getPublic();
//        PrivateKey privateKey1 = keyPair1.getPrivate();
//        System.out.println(Hex.encodeHexString(publicKey1.getEncoded()));
//        System.out.println(Hex.encodeHexString(privateKey1.getEncoded()));

//        生成公钥私钥2
//        KeyPair keyPair2 = getKeyPair();
//        PublicKey publicKey2 = keyPair2.getPublic();
//        PrivateKey privateKey2 = keyPair2.getPrivate();
//        System.out.println(Hex.encodeHexString(publicKey2.getEncoded()));
//        System.out.println(Hex.encodeHexString(privateKey2.getEncoded()));

        //生成多次的share key一样
//        for (int i = 0; i < 10; i++) {
//            String sharedKey1 = genSharedKey(publicKey1, privateKey2);
//            String sharedKey2 = genSharedKey(publicKey2, privateKey1);
//            System.out.println(sharedKey1);
//            System.out.println(sharedKey2);
//        }


    }

    //加签
    public static String signECDSA(PrivateKey privateKey, String message) {
        String result = "";
        try {
            //执行签名
            Signature signature = Signature.getInstance("SHA1withECDSA");
            signature.initSign(privateKey);
            signature.update(message.getBytes());
            byte[] sign = signature.sign();
            return Hex.encodeHexString(sign);
        } catch (Exception e) {
            e.printStackTrace();
        }
        return result;
    }

    //验签
    public static boolean verifyECDSA(PublicKey publicKey, String signed, String message) {
        try {
            //验证签名
            Signature signature = Signature.getInstance("SHA1withECDSA");
            signature.initVerify(publicKey);
            signature.update(message.getBytes());

            byte[] hex = Hex.decodeHex(signed);
            boolean bool = signature.verify(hex);
            System.out.println("验证：" + bool);
            return bool;
        } catch (Exception e) {
            e.printStackTrace();
        }
        return false;
    }

    /**
     * 从string转private key
     */
    public static PrivateKey getPrivateKey(String key) throws Exception {
        byte[] bytes = DatatypeConverter.parseHexBinary(key);

        PKCS8EncodedKeySpec keySpec = new PKCS8EncodedKeySpec(bytes);
        KeyFactory keyFactory = KeyFactory.getInstance("EC");
        return keyFactory.generatePrivate(keySpec);
    }

    /**
     * 从string转public key
     */
    public static PublicKey getPublicKey(String key) throws Exception {
        byte[] bytes = DatatypeConverter.parseHexBinary(key);

        X509EncodedKeySpec keySpec = new X509EncodedKeySpec(bytes);
        KeyFactory keyFactory = KeyFactory.getInstance("EC");
        return keyFactory.generatePublic(keySpec);
    }


    /**
     * 生成 share key
     *
     * @param publicStr  公钥字符串
     * @param privateStr 私钥字符串
     * @return
     */
    public static String genSharedKey(String publicStr, String privateStr) {
        try {
            return genSharedKey(getPublicKey(publicStr), getPrivateKey(privateStr));
        } catch (Exception e) {
            e.printStackTrace();
        }
        return null;
    }

    /**
     * 生成 share key
     *
     * @param publicKey  公钥
     * @param privateKey 私钥
     * @return
     */
    public static String genSharedKey(PublicKey publicKey, PrivateKey privateKey) {
        String sharedKey = "";
        try {
            KeyAgreement ka = KeyAgreement.getInstance("ECDH");
            ka.init(privateKey);
            ka.doPhase(publicKey, true);
            sharedKey = Hex.encodeHexString(ka.generateSecret());
        } catch (Exception e) {
            e.printStackTrace();
        }
        return sharedKey;
    }

    //生成KeyPair
    public static KeyPair getKeyPair() throws Exception {
        KeyPairGenerator keyGen = KeyPairGenerator.getInstance("EC");
        SecureRandom random = SecureRandom.getInstance("SHA1PRNG");
        keyGen.initialize(256, random);
        return keyGen.generateKeyPair();
    }


}


```

## 六、MD5
```
public class MD5Util {
    /**
     * 加密，不可逆
     *
     * @param password 需要加密的秘密
     * @return /
     */
    public static String encode(String password) {
        if (Objects.nonNull(password)) {
            try {
                MessageDigest md5 = MessageDigest.getInstance("MD5");
                // 生成 8位 的数字的数组，数组中有 16 个数
                byte[] digest = md5.digest(password.getBytes(StandardCharsets.UTF_8));
                // 将 16 个 8 位数组成一个 BigInteger 正整数，并转成 16 进制字符串输出
                return new BigInteger(1, digest).toString(16);
            } catch (NoSuchAlgorithmException e) {
                e.printStackTrace();
            }
        }
        return null;
    }

    /**
     * 校验密码是否正确
     *
     * @param password 输入的原始秘密
     * @param digest   存起来的加密后秘闻
     * @return /
     */
    public static boolean validate(String password, String digest) {
        if (Objects.nonNull(password) && Objects.nonNull(digest)) {
            return Objects.equals(encode(password), digest);
        }
        return false;
    }


    public static void main(String[] args) {
        String encode = MD5Util.encode("^23@oo723.!");
        System.out.println("encode:" + encode);
        boolean isEquals = MD5Util.validate("^23@oo723.!", "b656fc18a12e9b978d4bd967dd38eceb");
        System.out.println("isEquals:" + isEquals);
    }
}


```

## 七、PBE
```
public class PBEUtil {
    public static final String ALGORITHM = "PBEWITHMD5andDES";

    public static final int ITERATION_COUNT = 100;


    public static byte[] initSalt() throws Exception {
        //实例化安全随机数
        SecureRandom random = new SecureRandom();
        return random.generateSeed(8);
    }

    /***
     * 转换密钥
     * @param password 密码
     * @return 密钥
     * @throws Exception
     */
    private static Key toKey(String password) throws Exception {
        //密钥材料
        PBEKeySpec keySpec = new PBEKeySpec(password.toCharArray());
        //实例化
        SecretKeyFactory factory = SecretKeyFactory.getInstance(ALGORITHM);
        //生成密钥
        return factory.generateSecret(keySpec);
    }

    /***
     * 加密
     * @param data 待加密数据
     * @param password 密钥
     * @param salt
     * @return
     * @throws Exception
     */
    public static byte[] encrypt(byte[] data, String password, byte[] salt) throws Exception {
        //转换密钥
        Key key = toKey(password);
        //实例化PBE参数材料
        PBEParameterSpec spec = new PBEParameterSpec(salt, ITERATION_COUNT);
        //实例化
        Cipher cipher = Cipher.getInstance(ALGORITHM);
        //初始化
        cipher.init(Cipher.ENCRYPT_MODE, key, spec);
        return cipher.doFinal(data);
    }


    /***
     * 解密
     * @param data 待解密数据
     * @param password 密钥
     * @param salt
     * @return
     * @throws Exception
     */
    public static byte[] decrypt(byte[] data, String password, byte[] salt) throws Exception {
        //转换密钥
        Key key = toKey(password);
        //实例化PBE参数材料
        PBEParameterSpec spec = new PBEParameterSpec(salt, ITERATION_COUNT);
        //实例化
        Cipher cipher = Cipher.getInstance(ALGORITHM);
        //初始化
        cipher.init(Cipher.DECRYPT_MODE, key, spec);
        //执行操作
        return cipher.doFinal(data);
    }


    private static String showByteArray(byte[] data) {
        if (null == data) {
            return null;
        }
        StringBuilder sb = new StringBuilder();
        for (byte b : data) {
            sb.append(b).append(",");
        }
        sb.deleteCharAt(sb.length() - 1);
        sb.append("");
        return sb.toString();
    }


    public static void main(String[] args) throws Exception {
        byte[] salt = initSalt();
        System.out.println("salt：" + showByteArray(salt));
        String password = "j7&23/2!r/>n)";
        System.out.println("口令：" + password);
        String data = "PBE JAVA";
        System.out.println("加密前数据：String:" + data);
        System.out.println("加密前数据：byte[]:" + showByteArray(data.getBytes()));

        byte[] encryptData = encrypt(data.getBytes(), password, salt);
        System.out.println("加密后数据：byte[]:" + showByteArray(encryptData));

        byte[] decryptData = decrypt(encryptData, password, salt);
        System.out.println("解密后数据: byte[]:" + showByteArray(decryptData));
        System.out.println("解密后数据: string:" + new String(decryptData));
    }

}


```

## 八、RC4
```
public class RC4Util {
    /**
     * ACCESS_TOKEN的加密钥匙
     **/
    public static final String ACCESSKEY = "234.X!431243RTQWEACCESSKEY";


    /**
     * 加密
     **/
    public static String encrypt(String data, String key) {
        if (data == null || key == null) {
            return null;
        }
        return toHexString(asString(encrypt_byte(data, key)));
    }

    /**
     * 解密
     **/
    public static String decrypt(String data, String key) {
        if (data == null || key == null) {
            return null;
        }
        return new String(RC4Base(HexString2Bytes(data), key));
    }

    /**
     * 加密字节码
     **/
    public static byte[] encrypt_byte(String data, String key) {
        if (data == null || key == null) {
            return null;
        }
        byte b_data[] = data.getBytes();
        return RC4Base(b_data, key);
    }

    private static String asString(byte[] buf) {
        StringBuffer strbuf = new StringBuffer(buf.length);
        for (int i = 0; i < buf.length; i++) {
            strbuf.append((char) buf[i]);
        }
        return strbuf.toString();
    }

    private static byte[] initKey(String aKey) {
        byte[] b_key = aKey.getBytes();
        byte state[] = new byte[256];

        for (int i = 0; i < 256; i++) {
            state[i] = (byte) i;
        }
        int index1 = 0;
        int index2 = 0;
        if (b_key == null || b_key.length == 0) {
            return null;
        }
        for (int i = 0; i < 256; i++) {
            index2 = ((b_key[index1] & 0xff) + (state[i] & 0xff) + index2) & 0xff;
            byte tmp = state[i];
            state[i] = state[index2];
            state[index2] = tmp;
            index1 = (index1 + 1) % b_key.length;
        }
        return state;
    }

    private static String toHexString(String s) {
        String str = "";
        for (int i = 0; i < s.length(); i++) {
            int ch = (int) s.charAt(i);
            String s4 = Integer.toHexString(ch & 0xFF);
            if (s4.length() == 1) {
                s4 = '0' + s4;
            }
            str = str + s4;
        }
        return str;// 0x表示十六进制
    }

    private static byte[] HexString2Bytes(String src) {
        int size = src.length();
        byte[] ret = new byte[size / 2];
        byte[] tmp = src.getBytes();
        for (int i = 0; i < size / 2; i++) {
            ret[i] = uniteBytes(tmp[i * 2], tmp[i * 2 + 1]);
        }
        return ret;
    }

    private static byte uniteBytes(byte src0, byte src1) {
        char _b0 = (char) Byte.decode("0x" + new String(new byte[]{src0})).byteValue();
        _b0 = (char) (_b0 << 4);
        char _b1 = (char) Byte.decode("0x" + new String(new byte[]{src1})).byteValue();
        byte ret = (byte) (_b0 ^ _b1);
        return ret;
    }

    private static byte[] RC4Base(byte[] input, String mKkey) {
        int x = 0;
        int y = 0;
        byte key[] = initKey(mKkey);
        int xorIndex;
        byte[] result = new byte[input.length];

        for (int i = 0; i < input.length; i++) {
            x = (x + 1) & 0xff;
            y = ((key[x] & 0xff) + y) & 0xff;
            byte tmp = key[x];
            key[x] = key[y];
            key[y] = tmp;
            xorIndex = ((key[x] & 0xff) + (key[y] & 0xff)) & 0xff;
            result[i] = (byte) (input[i] ^ key[xorIndex]);
        }
        return result;
    }

    /**
     * 字符串转换成十六进制字符串
     */
    public static String str2HexStr(String str) {
        char[] chars = "0123456789ABCDEF".toCharArray();
        StringBuilder sb = new StringBuilder("");
        byte[] bs = str.getBytes();
        int bit;
        for (int i = 0; i < bs.length; i++) {
            bit = (bs[i] & 0x0f0) >> 4;
            sb.append(chars[bit]);
            bit = bs[i] & 0x0f;
            sb.append(chars[bit]);
        }
        return sb.toString();
    }

    /**
     * 十六进制转换字符串
     */

    public static String hexStr2Str(String hexStr) {
        String str = "0123456789ABCDEF";
        char[] hexs = hexStr.toCharArray();
        byte[] bytes = new byte[hexStr.length() / 2];
        int n;
        for (int i = 0; i < bytes.length; i++) {
            n = str.indexOf(hexs[2 * i]) * 16;
            n += str.indexOf(hexs[2 * i + 1]);
            bytes[i] = (byte) (n & 0xff);
        }
        return new String(bytes);
    }

    public static void main(String[] args) {
        String data = "Java RC4";
        System.out.println("加密结果 " + RC4Util.encrypt(data, ACCESSKEY));
        System.out.println("解密结果 " + RC4Util.decrypt(RC4Util.encrypt(data, ACCESSKEY), ACCESSKEY));
    }

}


```

## 九、RSA
```
public class RSAUtil {
    private static Map<Integer, String> keyMap = new HashMap<Integer, String>();  //用于封装随机产生的公钥与私钥

    public static void main(String[] args) throws Exception {
        //生成公钥和私钥
        genKeyPair();
        //加密字符串
        String message = "Java RSA";
        System.out.println("随机生成的公钥为:" + keyMap.get(0));
        System.out.println("随机生成的私钥为:" + keyMap.get(1));
        String messageEn = encrypt(message, keyMap.get(0));
        System.out.println(message + "\t加密后的字符串为:" + messageEn);
        String messageDe = decrypt(messageEn, keyMap.get(1));
        System.out.println("还原后的字符串为:" + messageDe);
    }

    /**
     * 随机生成密钥对
     *
     * @throws NoSuchAlgorithmException
     */
    public static void genKeyPair() throws NoSuchAlgorithmException {
        // KeyPairGenerator类用于生成公钥和私钥对，基于RSA算法生成对象
        KeyPairGenerator keyPairGen = KeyPairGenerator.getInstance("RSA");
        // 初始化密钥对生成器，密钥大小为96-1024位
        keyPairGen.initialize(1024, new SecureRandom());
        // 生成一个密钥对，保存在keyPair中
        KeyPair keyPair = keyPairGen.generateKeyPair();
        RSAPrivateKey privateKey = (RSAPrivateKey) keyPair.getPrivate();   // 得到私钥
        RSAPublicKey publicKey = (RSAPublicKey) keyPair.getPublic();  // 得到公钥
        String publicKeyString = new String(Base64.encodeBase64(publicKey.getEncoded()));
        // 得到私钥字符串
        String privateKeyString = new String(Base64.encodeBase64((privateKey.getEncoded())));
        // 将公钥和私钥保存到Map
        keyMap.put(0, publicKeyString);  //0表示公钥
        keyMap.put(1, privateKeyString);  //1表示私钥
    }

    /**
     * RSA公钥加密
     *
     * @param str       加密字符串
     * @param publicKey 公钥
     * @return 密文
     * @throws Exception 加密过程中的异常信息
     */
    public static String encrypt(String str, String publicKey) throws Exception {
        //base64编码的公钥
        byte[] decoded = Base64.decodeBase64(publicKey);
        RSAPublicKey pubKey = (RSAPublicKey) KeyFactory.getInstance("RSA").generatePublic(new X509EncodedKeySpec(decoded));
        //RSA加密
        Cipher cipher = Cipher.getInstance("RSA");
        cipher.init(Cipher.ENCRYPT_MODE, pubKey);
        String outStr = Base64.encodeBase64String(cipher.doFinal(str.getBytes("UTF-8")));
        return outStr;
    }

    /**
     * RSA私钥解密
     *
     * @param str        加密字符串
     * @param privateKey 私钥
     * @return 铭文
     * @throws Exception 解密过程中的异常信息
     */
    public static String decrypt(String str, String privateKey) throws Exception {
        //64位解码加密后的字符串
        byte[] inputByte = Base64.decodeBase64(str.getBytes("UTF-8"));
        //base64编码的私钥
        byte[] decoded = Base64.decodeBase64(privateKey);
        RSAPrivateKey priKey = (RSAPrivateKey) KeyFactory.getInstance("RSA").generatePrivate(new PKCS8EncodedKeySpec(decoded));
        //RSA解密
        Cipher cipher = Cipher.getInstance("RSA");
        cipher.init(Cipher.DECRYPT_MODE, priKey);
        String outStr = new String(cipher.doFinal(inputByte));
        return outStr;
    }

}


```
## 十、3DES
```
public class ThreeDesUtil {
    private static final String Algorithm = "DESede";

    public static void main(String[] args) throws Exception {
        String content = "Java Three DES";
        String originKey = "@.!456JJKL2#$XLFDQ..!@#4324";
        String cipherText = desEncript(content, originKey);
        System.out.println("通过3DES加密后的结果是：" + cipherText);
        String clearText1 = desDecript(cipherText, originKey);
        System.out.println("解密结果是：\n" + clearText1);
    }

    /**
     * 加密算法
     *
     * @param clearText
     * @param originKey
     * @return
     * @throws NoSuchAlgorithmException
     * @throws NoSuchPaddingException
     * @throws InvalidKeyException
     * @throws BadPaddingException
     * @throws IllegalBlockSizeException
     */
    private static String desEncript(String clearText, String originKey) throws NoSuchAlgorithmException, NoSuchPaddingException, InvalidKeyException, BadPaddingException, IllegalBlockSizeException {
        Cipher cipher = Cipher.getInstance(Algorithm); /*提供加密的方式：DES*/
        SecretKeySpec key = getKey(originKey);  /*对密钥进行操作，产生16个48位长的子密钥*/
        cipher.init(Cipher.ENCRYPT_MODE, key); /*初始化cipher，选定模式，这里为加密模式，并同时传入密钥*/
        byte[] doFinal = cipher.doFinal(clearText.getBytes());   /*开始加密操作*/
        String encode = Base64.encode(doFinal);    /*对加密后的数据按照Base64进行编码*/
        return encode;
    }

    /**
     * 解密算法
     *
     * @param cipherText
     * @param originKey
     * @return
     * @throws BadPaddingException
     * @throws IllegalBlockSizeException
     * @throws NoSuchPaddingException
     * @throws NoSuchAlgorithmException
     * @throws InvalidKeyException
     */
    private static String desDecript(String cipherText, String originKey) throws BadPaddingException, IllegalBlockSizeException, NoSuchPaddingException, NoSuchAlgorithmException, InvalidKeyException {
        Cipher cipher = Cipher.getInstance(Algorithm);   /*初始化加密方式*/
        Key key = getKey(originKey);  /*获取密钥*/
        cipher.init(Cipher.DECRYPT_MODE, key);  /*初始化操作方式*/
        byte[] decode = Base64.decode(cipherText);  /*按照Base64解码*/
        byte[] doFinal = cipher.doFinal(decode);   /*执行解码操作*/
        return new String(doFinal);   /*转换成相应字符串并返回*/
    }

    /**
     * 获取密钥算法
     *
     * @param originKey
     * @return
     */
    private static SecretKeySpec getKey(String originKey) {
        byte[] buffer = new byte[24];
        byte[] originBytes = originKey.getBytes();
        /**
         * 防止输入的密钥长度超过192位
         */
        for (int i = 0; i < 24 && i < originBytes.length; i++) {
            buffer[i] = originBytes[i];  /*如果originBytes不足8,buffer剩余的补零*/
        }
        SecretKeySpec key = new SecretKeySpec(buffer, Algorithm); /*第一个参数是密钥字节数组，第二个参数是加密方式*/
        return key;  /*返回操作之后得到的密钥*/
    }

}


```