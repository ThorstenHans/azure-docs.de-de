---
title: Konfigurieren von Windows-PHP-Apps
description: Erfahren Sie, wie Sie eine PHP-App in den nativen Windows-Instanzen von App Service konfigurieren. In diesem Artikel werden die gängigsten Konfigurationsaufgaben vorgestellt.
ms.devlang: php
ms.topic: article
ms.date: 06/02/2020
ms.openlocfilehash: 1eb4e9d349fdd0097cbde4e4cef3d5c61a167193
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84907922"
---
# <a name="configure-a-windows-php-app-for-azure-app-service"></a>Konfigurieren einer Windows-PHP-App für Azure App Service

In diesem Leitfaden erfahren Sie, wie Sie Ihre PHP-Web-Apps, mobilen Back-Ends und API-Apps in Azure App Service konfigurieren. Diese Anleitung gilt für Apps, die auf der Windows-Plattform gehostet werden. Informationen zu Linux-Apps finden Sie unter [Konfigurieren einer Linux-PHP-App für Azure App Service](containers/configure-language-php.md).

Dieser Leitfaden enthält die wichtigsten Konzepte und Anweisungen für PHP-Entwickler, die Apps in App Service bereitstellen. Wenn Sie Azure App Service noch nie verwendet haben, befolgen Sie zunächst den [PHP-Schnellstart](app-service-web-get-started-php.md) und das [Tutorial zu PHP mit MySQL](app-service-web-tutorial-php-mysql.md).

## <a name="show-php-version"></a>Anzeigen der PHP-Version

Führen Sie in [Cloud Shell](https://shell.azure.com) den folgenden Befehl aus, um die aktuelle PHP-Version anzuzeigen:

```azurecli-interactive
az webapp config show --resource-group <resource-group-name> --name <app-name> --query phpVersion
```

Führen Sie in [Cloud Shell](https://shell.azure.com) den folgenden Befehl aus, um alle unterstützten PHP-Versionen anzuzeigen:

```azurecli-interactive
az webapp list-runtimes | grep php
```

## <a name="set-php-version"></a>Festlegen der PHP-Version

Führen Sie in [Cloud Shell](https://shell.azure.com) den folgenden Befehl aus, um die PHP-Version auf 7.4 festzulegen:

```azurecli-interactive
az webapp config set --name <app-name> --resource-group <resource-group-name> --php-version 7.4
```

## <a name="run-composer"></a>Ausführen von Composer

Wenn Sie möchten, dass App Service zum Zeitpunkt der Bereitstellung [Composer](https://getcomposer.org/) ausführt, ist es am einfachsten, den Composer in Ihr Repository aufzunehmen.

Wechseln Sie in einem lokalen Terminalfenster zum Stammverzeichnis Ihres Repositorys, und folgen Sie den Anweisungen unter [Composer herunterladen](https://getcomposer.org/download/), um *composer.phar* in das Stammverzeichnis herunterzuladen.

Führen Sie die folgenden Befehle aus (wofür [npm](https://www.npmjs.com/get-npm) installiert sein muss):

```bash
npm install kuduscript -g
kuduscript --node --scriptType bash --suppressPrompt
```

Das Stammverzeichnis Ihres Repositorys enthält jetzt zwei zusätzliche Dateien: *.deployment* und *deploy.sh*.

Öffnen Sie *deploy.sh* und suchen Sie den Abschnitt `Deployment`, der wie folgt aussieht:

```bash
##################################################################################################################################
# Deployment
# ----------
```

Fügen Sie den Codeabschnitt, den Sie zum Ausführen des erforderlichen Tools benötigen, *am Ende* des Abschnitts `Deployment` hinzu:

```bash
# 4. Use composer
echo "$DEPLOYMENT_TARGET"
if [ -e "$DEPLOYMENT_TARGET/composer.json" ]; then
  echo "Found composer.json"
  pushd "$DEPLOYMENT_TARGET"
  php composer.phar install $COMPOSER_ARGS
  exitWithMessageOnError "Composer install failed"
  popd
fi
```

Committen Sie alle Änderungen, und stellen Sie Ihren Code mithilfe der Git- oder ZIP-Bereitstellung mit aktivierter Buildautomatisierung bereit. Composer sollte jetzt als Teil der Bereitstellungsautomatisierung ausgeführt werden.

## <a name="run-gruntbowergulp"></a>Run Grunt/Bower/Gulp

Wenn App Service zum Bereitstellungszeitpunkt eines der beliebten Automationstools wie Grunt, Bower oder Gulp ausführen soll, müssen Sie ein [benutzerdefiniertes Bereitstellungsskript](https://github.com/projectkudu/kudu/wiki/Custom-Deployment-Script) angeben. App Service führt dieses Skript bei der Git- oder [ZIP-Bereitstellung](deploy-zip.md) mit aktivierter Buildautomatisierung aus. 

Damit Ihr Repository diese Tools ausführen kann, müssen Sie sie zu den Abhängigkeiten in *package.json* hinzufügen. Beispiel:

```json
"dependencies": {
  "bower": "^1.7.9",
  "grunt": "^1.0.1",
  "gulp": "^3.9.1",
  ...
}
```

Wechseln Sie in einem lokalen Terminalfenster zum Stammverzeichnis Ihres Repositorys, und führen Sie die folgenden Befehle aus (wofür [npm](https://www.npmjs.com/get-npm) installiert sein muss):

```bash
npm install kuduscript -g
kuduscript --node --scriptType bash --suppressPrompt
```

Das Stammverzeichnis Ihres Repositorys enthält jetzt zwei zusätzliche Dateien: *.deployment* und *deploy.sh*.

Öffnen Sie *deploy.sh* und suchen Sie den Abschnitt `Deployment`, der wie folgt aussieht:

```bash
##################################################################################################################################
# Deployment
# ----------
```

Dieser Abschnitt endet mit dem Ausführen von `npm install --production`. Fügen Sie den Codeabschnitt, den Sie zum Ausführen des erforderlichen Tools benötigen, *am Ende* des Abschnitts `Deployment` hinzu:

- [Bower](#bower)
- [Gulp](#gulp)
- [Grunt](#grunt)

Weitere Informationen finden Sie in einem [Beispiel im der MEAN.js-Beispiel](https://github.com/Azure-Samples/meanjs/blob/master/deploy.sh#L112-L135), in dem das Bereitstellungsskript auch einen benutzerdefinierten `npm install`-Befehl ausführt.

### <a name="bower"></a>Bower

Dieser Codeausschnitt führt `bower install` aus.

```bash
if [ -e "$DEPLOYMENT_TARGET/bower.json" ]; then
  cd "$DEPLOYMENT_TARGET"
  eval ./node_modules/.bin/bower install
  exitWithMessageOnError "bower failed"
  cd - > /dev/null
fi
```

### <a name="gulp"></a>Gulp

Dieser Codeausschnitt führt `gulp imagemin` aus.

```bash
if [ -e "$DEPLOYMENT_TARGET/gulpfile.js" ]; then
  cd "$DEPLOYMENT_TARGET"
  eval ./node_modules/.bin/gulp imagemin
  exitWithMessageOnError "gulp failed"
  cd - > /dev/null
fi
```

### <a name="grunt"></a>Grunt

Dieser Codeausschnitt führt `grunt` aus.

```bash
if [ -e "$DEPLOYMENT_TARGET/Gruntfile.js" ]; then
  cd "$DEPLOYMENT_TARGET"
  eval ./node_modules/.bin/grunt
  exitWithMessageOnError "Grunt failed"
  cd - > /dev/null
fi
```

## <a name="access-environment-variables"></a>Zugreifen auf Umgebungsvariablen

In App Service können Sie [App-Einstellungen außerhalb Ihres App-Codes festlegen](configure-common.md#configure-app-settings). Anschließend können Sie darauf unter Verwendung des Standardmusters [getenv()](https://secure.php.net/manual/function.getenv.php) zugreifen. Verwenden Sie beispielsweise den folgenden Code, um auf eine App-Einstellung namens `DB_HOST` zuzugreifen:

```php
getenv("DB_HOST")
```

## <a name="change-site-root"></a>Ändern des Sitestammverzeichnisses

Das Webframework Ihrer Wahl verwendet möglicherweise ein Unterverzeichnis als Stammverzeichnis der Site. Beispielsweise verwendet [Laravel](https://laravel.com/) das Unterverzeichnis *public/* als Stammverzeichnis der Website.

Um das Stammverzeichnis der Website anzupassen, legen Sie den virtuellen Anwendungspfad für die App mit dem Befehl [`az resource update`](/cli/azure/resource#az-resource-update) fest. Im folgenden Beispiel wird das Stammverzeichnis der Website auf das Unterverzeichnis *public/* in Ihrem Repository festgelegt. 

```azurecli-interactive
az resource update --name web --resource-group <group-name> --namespace Microsoft.Web --resource-type config --parent sites/<app-name> --set properties.virtualApplications[0].physicalPath="site\wwwroot\public" --api-version 2015-06-01
```

Standardmäßig verweist Azure App Service im Stammverzeichnis des virtuellen Anwendungspfads ( _/_ ) auf das Stammverzeichnis der bereitgestellten Anwendungsdateien (_sites\wwwroot_).

## <a name="detect-https-session"></a>Erkennen einer HTTPS-Sitzung

In App Service erfolgt die [SSL-Terminierung](https://wikipedia.org/wiki/TLS_termination_proxy) in den Modulen für den Netzwerklastenausgleich, sodass alle HTTPS-Anforderungen Ihre App als unverschlüsselte HTTP-Anforderungen erreichen. Wenn Ihre App-Logik überprüfen muss, ob Benutzeranforderungen verschlüsselt sind, können Sie dazu den Header `X-Forwarded-Proto` untersuchen.

```php
if (isset($_SERVER['X-Forwarded-Proto']) && $_SERVER['X-Forwarded-Proto'] === 'https') {
  // Do something when HTTPS is used
}
```

Gängige Webframeworks ermöglichen den Zugriff auf die Information `X-Forwarded-*` in Ihrem App-Standardmuster. In [CodeIgniter](https://codeigniter.com/) überprüft [is_https()](https://github.com/bcit-ci/CodeIgniter/blob/master/system/core/Common.php#L338-L365) standardmäßig den Wert von `X_FORWARDED_PROTO`.

## <a name="customize-phpini-settings"></a>Anpassen der „php.ini“-Einstellungen

Wenn Sie Änderungen an Ihrer PHP-Installation vornehmen müssen, können Sie jede der [php.ini-Anweisungen](https://www.php.net/manual/ini.list.php) mit folgenden Schritten ändern.

> [!NOTE]
> Die beste Möglichkeit zum Anzeigen der PHP-Version und der aktuellen *php.ini*-Konfiguration besteht darin, [phpinfo()](https://php.net/manual/function.phpinfo.php) in Ihrer App aufzurufen.
>

### <a name="customize-non-php_ini_system-directives"></a><a name="Customize-non-PHP_INI_SYSTEM directives"></a>Anpassen von Nicht-PHP_INI_SYSTEM-Anweisungen

Zum Anpassen der Anweisungen PHP_INI_USER PHP_INI_PERDIR und PHP_INI_ALL (siehe [php.ini-Anweisungen](https://www.php.net/manual/ini.list.php)), fügen Sie dem Stammverzeichnis Ihrer App eine `.user.ini`-Datei hinzu.

Fügen Sie der Datei `.user.ini` die Konfigurationseinstellungen hinzu, und verwenden Sie dieselbe Syntax wie für eine `php.ini`-Datei. Wenn Sie zum Beispiel die Einstellung `display_errors` aktivieren und die Einstellung `upload_max_filesize` auf 10 M festlegen möchten, enthält die Datei `.user.ini` diesen Text:

```
 ; Example Settings
 display_errors=On
 upload_max_filesize=10M

 ; Write errors to d:\home\LogFiles\php_errors.log
 ; log_errors=On
```

Stellen Sie Ihre App mit den Änderungen erneut bereit, und starten Sie sie neu.

Als Alternative zur Verwendung einer `.user.ini`-Datei können Sie [ini_set()](https://www.php.net/manual/function.ini-set.php) in Ihrer App verwenden, um diese Nicht-PHP_INI_SYSTEM-Anweisungen anzupassen.

### <a name="customize-php_ini_system-directives"></a><a name="customize-php_ini_system-directives"></a>Anpassen von PHP_INI_SYSTEM-Anweisungen

Zum Anpassen von PHP_INI_SYSTEM-Anweisungen (siehe [php.ini-Anweisungen](https://www.php.net/manual/ini.list.php)) können Sie nicht den Ansatz mit *.htaccess* verwenden. App Service bietet einen separaten Mechanismus, der die App-Einstellung `PHP_INI_SCAN_DIR` verwendet.

Führen Sie als Erstes den folgenden Befehl in der [Cloud Shell](https://shell.azure.com) aus, um eine App-Einstellung namens `PHP_INI_SCAN_DIR` hinzuzufügen:

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings PHP_INI_SCAN_DIR="d:\home\site\ini"
```

Navigieren Sie zur Kudu-Konsole (`https://<app-name>.scm.azurewebsites.net/DebugConsole`) und zu `d:\home\site`.

Erstellen Sie ein Verzeichnis in `d:\home\site` namens `ini`, erstellen Sie dann eine *.ini*-Datei im Verzeichnis `d:\home\site\ini` (z. B. *settings.ini)* mit den Anweisungen, die Sie anpassen möchten. Verwenden Sie dieselbe Syntax, die Sie in einer *php.ini*-Datei verwenden würden. 

Führen Sie beispielsweise zum Ändern des Werts von [expose_php](https://php.net/manual/ini.core.php#ini.expose-php) die folgenden Befehle aus:

```bash
cd /home/site
mkdir ini
echo "expose_php = Off" >> ini/setting.ini
```

Damit die Änderungen wirksam werden, starten Sie die App neu.

## <a name="enable-php-extensions"></a>Aktivieren von PHP-Erweiterungen

Die integrierten PHP-Installationen enthalten die am häufigsten verwendeten Erweiterungen. Sie können zusätzliche Erweiterungen auf dieselbe Weise aktivieren, wie Sie [php.ini-Anweisungen anpassen](#customize-php_ini_system-directives).

> [!NOTE]
> Die beste Möglichkeit zum Anzeigen der PHP-Version und der aktuellen *php.ini*-Konfiguration besteht darin, [phpinfo()](https://php.net/manual/function.phpinfo.php) in Ihrer App aufzurufen.
>

Um zusätzliche Erweiterungen zu aktivieren, führen Sie diese Schritte aus:

Fügen Sie im Stammverzeichnis Ihrer App ein Verzeichnis `bin` hinzu, und legen Sie die `.so`-Erweiterungsdateien darin ab (z. B. *mongodb.so*). Stellen Sie sicher, dass die Erweiterungen mit der PHP-Version in Azure sowie mit VC9 und nts-kompatibel (non-thread-safe, nicht threadsicher) sind.

Stellen Sie Ihre Änderungen bereit.

Befolgen Sie die Schritte in [Anpassen von PHP_INI_SYSTEM-Anweisungen](#customize-php_ini_system-directives), fügen Sie die Erweiterungen in der benutzerdefinierten *.ini*-Datei mit den Anweisungen [extension](https://www.php.net/manual/ini.core.php#ini.extension) oder [zend_extension](https://www.php.net/manual/ini.core.php#ini.zend-extension) hinzu.

```
extension=d:\home\site\wwwroot\bin\mongodb.so
zend_extension=d:\home\site\wwwroot\bin\xdebug.so
```

Damit die Änderungen wirksam werden, starten Sie die App neu.

## <a name="access-diagnostic-logs"></a>Zugreifen auf Diagnoseprotokolle

Verwenden Sie das Standardhilfsprogramm [error_log()](https://php.net/manual/function.error-log.php), um Ihre Diagnoseprotokolle in Azure App Service anzuzeigen.

[!INCLUDE [Access diagnostic logs](../../includes/app-service-web-logs-access-no-h.md)]

## <a name="troubleshooting"></a>Problembehandlung

Wenn sich eine funktionierende PHP-App in App Service anders verhält oder Fehler aufweist, versuchen Sie Folgendes:

- [Greifen Sie auf den Protokolldatenstrom zu](#access-diagnostic-logs).
- Testen Sie die App lokal im Produktionsmodus. App Service führt Ihre Apps im Produktionsmodus aus. Daher müssen Sie sicherstellen, dass Ihr Projekt lokal wie erwartet im Produktionsmodus funktioniert. Beispiel:
    - Bestimmte Webframeworks können statische Dateien im Produktionsmodus unterschiedlich bereitstellen.
    - Bestimmte Webframeworks können benutzerdefinierte Startskripts verwenden, wenn sie im Produktionsmodus ausgeführt werden.
- Führen Sie Ihre App in App Service im Debugmodus aus. Z. B. können Sie in [Laravel](https://meanjs.org/) Ihre App so konfigurieren, dass sie in der Produktion Debugmeldungen ausgibt, indem Sie [die App-Einstellung `APP_DEBUG` auf `true`](configure-common.md#configure-app-settings) festlegen.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Tutorial: PHP-App mit MySQL](app-service-web-tutorial-php-mysql.md)
