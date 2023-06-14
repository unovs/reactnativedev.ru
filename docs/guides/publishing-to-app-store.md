# Публикация в Apple App Store

Процесс публикации такой же, как и у любого другого нативного приложения для iOS, с некоторыми дополнительными соображениями, которые необходимо принять во внимание.

!!!info ""

    Если вы используете Expo, прочитайте руководство Expo [Deploying to App Stores](https://docs.expo.dev/distribution/app-stores/) для создания и отправки вашего приложения в Apple App Store. Это руководство работает с любым приложением React Native для автоматизации процесса развертывания.

## 1. Включите App Transport Security

App Transport Security - это функция безопасности, представленная в iOS 9, которая отклоняет все HTTP-запросы, отправленные не по HTTPS. Это может привести к блокировке HTTP-трафика, включая сервер разработчика React Native. ATS отключен для `localhost` по умолчанию в проектах React Native, чтобы упростить разработку.

Вы должны снова включить ATS перед сборкой приложения для производства, удалив запись `localhost` из словаря `NSExceptionDomains` и установив `NSAllowsArbitraryLoads` на `false` в файле `Info.plist` в папке `ios/`. Вы также можете повторно включить ATS из Xcode, открыв свойства вашей цели в панели Info и отредактировав запись App Transport Security Settings.

!!!note ""

    Если вашему приложению требуется доступ к HTTP-ресурсам на производстве, узнайте, как настроить ATS в вашем проекте.

## 2. Настройка схемы выпуска

Создание приложения для распространения в App Store требует использования схемы `Release` в Xcode. Приложения, созданные по схеме `Release`, будут автоматически отключать внутриприкладное меню Dev Menu, что предотвратит случайный доступ пользователей к меню в продакшене. Кроме того, JavaScript будет собираться локально, так что вы сможете установить приложение на устройство и протестировать его, не подключаясь к компьютеру.

Чтобы настроить приложение на сборку по схеме `Release`, перейдите в **Product** → **Scheme** → **Edit Scheme**. Выберите вкладку **Run** на боковой панели, затем установите в выпадающем списке Build Configuration значение `Release`.

![Настройка схемы выпуска](ConfigureReleaseScheme.png)

### Советы профессионалов

По мере увеличения размера вашего App Bundle вы можете начать видеть, как между заставкой и отображением представления корневого приложения появляется пустой экран. В этом случае вы можете добавить следующий код в `AppDelegate.m`, чтобы сохранить отображение заставки во время перехода.

```objectivec
  // Place this code after "[self.window makeKeyAndVisible]" and before "return YES;"
  UIStoryboard *sb = [UIStoryboard storyboardWithName:@"LaunchScreen" bundle:nil];
  UIViewController *vc = [sb instantiateInitialViewController];
  rootView.loadingView = vc.view;
```

Статический бандл создается каждый раз, когда вы нацеливаетесь на физическое устройство, даже в Debug. Если вы хотите сэкономить время, отключите генерацию пакета в Debug, добавив следующее в сценарий оболочки на этапе сборки Xcode `Bundle React Native code and images`:

```shell
 if [ "${CONFIGURATION}" == "Debug" ]; then
  export SKIP_BUNDLING=true
 fi
```

## 3. Сборка приложения для выпуска

Теперь вы можете создать приложение для выпуска, нажав `⌘B` или выбрав **Product** → **Build** в строке меню. После создания приложения для выпуска вы сможете распространить его среди бета-тестеров и отправить приложение в App Store.

!!!info ""

    Вы также можете использовать `React Native CLI` для выполнения этой операции, используя опцию `--mode` со значением `Release` (например, `npx react-native run-ios --mode Release`).

Когда вы закончите тестирование и будете готовы опубликовать приложение в App Store, следуйте этому руководству.

-   Запустите терминал, перейдите в папку iOS вашего приложения и введите `open .`.
-   Дважды щелкните на файле YOUR_APP_NAME.xcworkspace. Должен запуститься XCode.
-   Нажмите на `Продукт` → `Архив`. Убедитесь, что устройство установлено на "Any iOS Device (arm64)".

!!!note ""

    Проверьте идентификатор пакета и убедитесь, что он полностью совпадает с идентификатором, который вы создали в Identifiers в Apple Developer Dashboard.

-   После завершения архивации в окне архива нажмите на `Распространить приложение`.
-   Нажмите на `App Store Connect` (если вы хотите опубликовать приложение в App Store).
-   Нажмите `Upload` → Убедитесь, что все флажки установлены, нажмите `Next`.
-   Выберите между `Автоматически управлять подписанием` и `Вручную управлять подписанием` в зависимости от ваших потребностей.
-   Нажмите на `Выгрузить`.
-   Теперь вы можете найти его в App Store Connect в разделе TestFlight.

Теперь заполните необходимую информацию и в разделе Build Section выберите сборку приложения и нажмите на `Save` → `Submit For Review`.