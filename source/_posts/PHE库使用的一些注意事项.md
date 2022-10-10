---
mathjax: true
title: PHE库使用的一些注意事项
date: 2021-09-24 20:06:49
tags: 
- PHE库
categories:
- python
---

开源密码学库PHE

<!--more-->

# 库的结构

+ PHE
  + util 基本工具包
  
  + encoding 编码工具，将int或者float编码为可加密与可直接在密文上运算的结构
  
  + paillier 密钥生成与加密解密


# 密钥生成

## 函数功能说明

```python
def generate_paillier_keypair(private_keyring=None, n_length=DEFAULT_KEYSIZE):
    '''
    返回两个类
    '''
    """Return a new :class:`PaillierPublicKey` and :class:`PaillierPrivateKey`.

    Add the private key to *private_keyring* if given.

    Args:
      private_keyring (PaillierPrivateKeyring): a
        :class:`PaillierPrivateKeyring` on which to store the private
        key.
      n_length: key size in bits.

    Returns:
      tuple: The generated :class:`PaillierPublicKey` and
      :class:`PaillierPrivateKey`
    """
```



# 加密操作

# 密文上进行运算

