title: "iOS开发——MD5加密算法"
date: 2015-08-03 13:35:22
tags: MD5加密
---

1. MD5加密算法，实现类别如下：

	方法一：
	```
	#import <CommonCrypto/CommonDigest.h>
	
	@interface NSString (md5)
	-(NSString *)md5HexDigest;
	@end
	
	#improt "NSString+MD5HexDigest.h"
	
	@implementation NSString (md5)
	
	- (NSString *)md5HexDigest {
		const char *original_str = [self UTF8String];
		unsigned char result[CC_MD5_DIGEST_LENGTH];
		CC_MD5(original_str, strlen(original_str), result);
		NSMutableString *hash = [NSMutableString string];
		for (int i = 0;i < 16; i++) {
			[hash appendFormat:@"%02X", result[i]];
		}
		return [hash lowercaseString];
	}
	
	@end
	```

	方法二：
	将字符串进行MD5加密，返回加密后的字符串
	```
	#import <CommonCrypro/CommonDigest.h>
	
	- (NSString *)md5WithString:(NSString *)str
	{
		const char *cStr = [str UTF8String];
		unsigned char result[16];
		CC_MD5(cStr, strlen(cStr), result);// this is the md call
		return [NSString stringWithFormat:@"%02x%02x%02x%02x%02x%02x%02x%02x%02x%02x%02x%02x%02x%02x%02x%02x",  
		result[0], result[1], result[2], result[3], 
        result[4], result[5], result[6], result[7],
        result[8], result[9], result[10], result[11],
        result[12], result[13], result[14], result[15]];
	}
	```
	`注意：` MD5是不可逆的只有加密没有解密