SimpleTwitterReverseAuth
========================

By adding a few files and adding one line of code you can get a twitter user's oauth_token and oauth_token_secret.

Based of @theseancook's work  here https://github.com/seancook/TWReverseAuthExample

His project is the "Full doctrine", but this project allows you to import the important files from his project and add the following to get the oauth_token and oauth_token_secret. The major change I made was I made all the TWAPIManager functions public so that you don't have to make a instance of TWAPIManager to get the OAuth tokens. 

Import ABOAuthCore files and the 4 TW files into your xcode project.

In TWSignedRequest.m be sure to insert your Twitter app's Consumer Key and Consumer Secrect. Then delete this line:

```objective-c
#error Insert your consumer key and consumer secret below and then delete this line
```

The project will not run until that line is deleted.

Then in the class where you want to get a twitter user's oauth_token and oauth_token_secret import "TWAPIManager.h"

Then add the following line:
```objective-c
    [TWAPIManager performReverseAuthForAccount:tempAcc withHandler:^(NSData *responseData, NSError *error) {
         if (responseData) {
             NSString *responseStr = [[NSString alloc]
                                      initWithData:responseData
                                      encoding:NSUTF8StringEncoding];
             
             NSMutableDictionary *oauthValues = [[NSMutableDictionary alloc] init];
             
             NSArray *parts = [responseStr componentsSeparatedByString:@"&"];
             
             for (NSString *subpart in parts)
             {
                 NSArray *keyAndObject = [subpart componentsSeparatedByString:@"="];
                 [oauthValues setObject:keyAndObject[1] forKey:keyAndObject[0]];
             }
             
             self.oauthValues = oauthValues;
         }
         else {
             NSLog(@"Error!\n%@", [error localizedDescription]);
         }
     }];
```
The variable tempAcc is a varible I created earlier, you have to insert your own ACAccount here.

Also for my project, I had a class varible where I store the user's OAuth information in a NSDictionary. That's why assigned the NSMutableDictionary to self.oauthValues. You can handle this data however you'd like.

This is my first attempt at putting some of my work out in the open. Please leave any feedback and add input where you'd like.
