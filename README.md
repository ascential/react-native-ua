# React Native: Urban Airship Module

React Native module for [Urban Airship](http://docs.urbanairship.com) platforms: *iOS* and *Android*

### Pre-requisitos
- Ter o certificado de desenvolvimento IOS configurado no seu ambiente;
- Ter uma app configurada para as plataformas IOS e/ou Android no UrbanAirship.

### Installation
- Acesse seu projeto React Native e instale o módulo: `npm install react-native-ua --save`

#### iOS setup
- Abrir o projeto IOS da sua app React Native no Xcode;
- Habilitar suporte a notificação no projeto:
- - Dentro de *Capabilities*, habilitar **Push Notification**;
- - Dentro de *Capabilities* habilitar  **Remote notifications** em *Background Modes*.
- Adicionar [ReactNativeUAIOS.xcodeproj](https://github.com/globocom/react-native-ua/tree/master/ios/ReactNativeUAIOS.xcodeproj), existente na pasta `node_modules/react-native-ua/ios` da sua app, dentro das **Libraries** do seu projeto no Xcode.
- Criar o arquivo [AirshipConfig.plist](https://github.com/globocom/react-native-ua/blob/master/ios/AirshipConfig.plist) na raiz do seu projeto com os dados de configuração da sua app do urbanAirship;
- Configurar Bridge:
- - Em *Build Settings* inclua as flags `-ObjC`, `-lz` e `-lsqlite3` em **Other Linker Flags**;
- - Em *Build Settings* inclua o path `$(SRCROOT)/../node_modules/react-native-ua/ios` e selecione **recursive** em *Header Search Paths*;
- - Em *Build Phases* inclua a `libReactNativeUAIOS.a` do seu projeto em *Link Binary With Files*;
- - Em *Build Phaces* inclua o arquivo `AirshipResources.bundle` existente na pasta `node_modules/react-native-ua/ios/Libraries/Airship` em *Copy Bundle Resources*;
- Dentro do *AppDelegate.m*, importar o módulo [ReactNativeUAIOS.h](https://github.com/globocom/react-native-ua/blob/master/ios/ReactNativeUAIOS.h) e realizar a sua chamada `[ReactNativeUAIOS setupUrbanAirship];`:

```objective-c
#import "ReactNativeUAIOS.h"

// ...

@implementation AppDelegate

- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
{
  // setup react native urban airship
  [ReactNativeUAIOS setupUrbanAirship];

  // ...
}

// ...

@end
```

### Usage

``` javascript
import ReactNativeUA from 'react-native-ua'; // import module

ReactNativeUA.enable_notification(); // prompt user to enable notification 
ReactNativeUA.disable_notification(); // prompt user to disable notification
ReactNativeUA.add_tag("tag"); // add only one tag 
ReactNativeUA.remove_tag("tag"); // remove only one tag
ReactNativeUA.set_trags(["tag-a", "tag-b"]); // overwrite all tags

// add handler to handle all incoming notifications
ReactNativeUA.subscribe_to("receivedNotification", (notification) => {
  console.log(notification.type,
              notification.data.aps.alert,
              notification.data.link);
});

// remove already added handler
ReactNativeUA.unsubscribe_to("receivedNotification");
```
