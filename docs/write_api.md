# Write API V1

This document is part of the Adobe® PhoneGap™ Build API V1 documentation; see also:

* [API Overview](/docs/api)
* [Read API](/docs/read_api)

All write APIs expect JSON-encoded content. Many also accept file uploads. Because of this, we expect API requests to have the content type `multipart/form-data`, and JSON bodies of requests are expected to have the name `data`. We're looking for a more elegant way of dealing with this in a future release.

### POST https://build.phonegap.com/api/v1/apps

Create a new app.

#### Required parameters

* __title__: You must specify a title for your app - if a title is also specified in a `config.xml` in your package, the one in the `config.xml` file will take preference.
* __create_method__: How the app is created (described below). There are three valid values:
  * __file__: A file is being uploaded with the app content
  * __remote_repo__: You have a remote repository with your app content
  * __hosted_repo__: You wish to upload your app content to a git repository hosted on `git.phonegap.com`

#### Optional parameters

* __package__: Sets the package identifier for your app. This can also be done after creation, or in your `config.xml` file. Defaults to `com.phonegap.www`
* __version__: Sets the version of your app. This can also be done after creation, or in your `config.xml` file. Defaults to `0.0.1`
* __description__: Sets the description for your app. This can also be done after creation, or in your `config.xml` file. Defaults to empty.
* __debug__: Builds your app in [debug mode](/docs/phonegap-debug). Defaults to false.
* __keys__: Set the signing keys to use for each platform you wish to sign. See below for more details
* __private__: Whether your app can be publicly downloaded. Defaults to `true` during beta period; will default to `false` once the beta period is complete
* __phonegap_version__: Which version of PhoneGap your app uses. See [config.xml](/docs/config-xml) for details on which are supported, and which one is currently the default

#### create_method

A new app can be created from an archive file or a remote git repository, or a PhoneGap Build hosted git repository can be created along with the application. You can choose which one of these to use by setting the `create_method` parameter in your JSON data.

The create method is immutable - an app that is created from a repository can never be changed to be file-backed, or vice versa. If you want to change at some later date, delete the old app and create a new one.

#### File-backed applications

To create a file-backed application, set the `create_method` parameter to `file`, and include a zip file, a tar.gz file, or an index.html file in the multipart body of your post, with the parameter name `file`.

<pre><strong>$ curl -F file=@/Users/alunny/index.html -u andrew.lunny@nitobi.com -F 'data={"title":"API V1 App","package":"com.alunny.apiv1","version":"0.1.0","create_method":"file"}' https://build.phonegap.com/api/v1/apps</strong></pre>
    {
        "keys":{
            "ios":{
                "title":"ios-key",
                "default":true,
                "id":2,
                "link":"/api/v1/keys/ios/2"
             },
             "blackberry":null,
             "android":{
                "title":"release-key",
                "default":true,
                "id":2,
                "link":"/api/v1/keys/android/2"
             }
        },
        "download":{},
        "title":"API V1 App",
        "repo":null,
        "collaborators":[
            {
                "person":"andrew.lunny@nitobi.com",
                "role":"admin"
            }
        ],
        "role":"admin",
        "id":26486,
        "icon":{
            "filename":null,
            "link":"/api/v1/apps/26486/icon"
        },
        "package":"com.alunny.apiv1",
        "version":"0.1.0",
        "description":null,
        "debug":false,
        "private":true,
        "link":"/api/v1/apps/26486",
        "status":{
            "webos":"pending",
            "ios":"pending",
            "blackberry":"pending",
            "android":"pending",
            "symbian":"pending",
            "winphone":"pending"
        },
        "error":{},
        "phonegap_version":"1.3.0",
        "build_count":null
    }

#### Remote-repository backed applications

To create an app based on a remote repo, set the `create_method` parameter to `remote_repo`, and include a `repo` parameter with the URL of the repository.

The URL has to be publicly accessible: PhoneGap Build will not authenticate against your repository. If you wish to keep your code private, use one of the other `create_method` options.

<pre><strong>$ curl -u andrew.lunny@nitobi.com -d 'data={"title":"API V1 App","repo":"https://github.com/alunny/phonegap-start.git","create_method":"remote_repo"}' https://build.phonegap.com/api/v1/apps</strong></pre>
    {
        "keys":{
            "ios":{
                "title":"ios-key",
                "default":true,
                "id":2,
                "link":"/api/v1/keys/ios/2"
             },
             "blackberry":null,
             "android":{
                "title":"release-key",
                "default":true,
                "id":2,
                "link":"/api/v1/keys/android/2"
             }
        },
        "download":{},
        "title":"alunnys Amazing App",
        "repo":"https://github.com/alunny/phonegap-start.git",
        "collaborators":[
            {
                "person":"andrew.lunny@nitobi.com",
                "role":"admin"
            }
        ],
        "role":"admin",
        "id":26488,
        "icon":{
            "filename":"blurry",
            "link":"/api/v1/apps/26488/icon"
        },
        "package":null,
        "version":null,
        "description":null,
        "debug":false,
        "private":true,
        "link":"/api/v1/apps/26488,
        "status":{
            "webos":"pending",
            "ios":"pending",
            "blackberry":"pending",
            "android":"pending",
            "symbian":"pending"
        },
        "error":{},
        "phonegap_version":"1.3.0",
        "build_count":null
    }

If you provide a repository URL that requires authentication, the response will have a `400` HTTP status code and the error message in the body of the response:

<pre><strong>$ curl -u andrew.lunny@nitobi.com -d 'data={"title":"API V1 App","repo":"https://alunny@github.com/alunny/phonegap-start.git","create_method":"remote_repo"}' https://build.phonegap.com/api/v1/apps</strong></pre>
    {
        "error":"Private repository URLs not supported - try removing &quot;alunny@&quot;"
    }

#### PhoneGap Build Hosted Repositories

To request a PhoneGap Build hosted repository for your application, set the `create_method` parameter to `hosted_repo`.

Once you have received the JSON response, you can read the `repo` attribute of the app, and push to that git remote to get your application building. See [our article on Git hosting](/docs/git-hosting) for more information on using PhoneGap Build's authenticated git repositories.

<pre><strong>$ curl -u andrew.lunny@nitobi.com -d 'data={"title":"Hosted Application","create_method":"hosted_repo"}' https://build.phonegap.com/api/v1/apps</strong></pre>
    {
        "keys":{
            "ios":{
                "title":"ios-key",
                "default":true,
                "id":2,
                "link":"/api/v1/keys/ios/2"
             },
             "blackberry":null,
             "android":{
                "title":"release-key",
                "default":true,
                "id":2,
                "link":"/api/v1/keys/android/2"
             }
        },
        "download":{},
        "title":"Hosted Application",
        "repo":"git@git.phonegap.com:alunny/26500_HostedApplication.git",
        "collaborators":[
            {
                "person":"andrew.lunny@nitobi.com",
                "role":"admin"
            }
        ],
        "role":"admin",
        "id":26500,
        "icon":{
            "filename":"null",
            "link":"/api/v1/apps/26500/icon"
        },
        "package":null,
        "version":null,
        "description":null,
        "debug":false,
        "private":true,
        "link":"/api/v1/apps/26500,
        "status":{
            "webos":"pending",
            "ios":"pending",
            "blackberry":"pending",
            "android":"pending",
            "symbian":"pending"
        },
        "error":{},
        "phonegap_version":"1.3.0",
        "build_count":null
    }

#### Signing keys

To sign your builds on PhoneGap Build, you must first upload one or more keys, through the `POST https://build.phonegap.com/api/v1/keys` method, or through the web interface. You can get a list of all the keys associated with your account by sending a GET request to that same URL.

In the `data` JSON hash that you send to the build server, you can specify the keys, per platform, by id, that you wish to use to build. Here is a sample post:

<pre><strong>$ curl -u andrew.lunny@nitobi.com -d 'data={"title":"Signing Keys","create_method":"hosted_repo","keys"{"ios":123,"android":567}}' https://build.phonegap.com/api/v1/apps</strong></pre>
    {
        "keys":{
            "ios":{
                "title":"new iOS key",
                "default":false,
                "id":123,
                "link":"/api/v1/keys/ios/123"
             },
             "blackberry":null,
             "android":{
                "title":"some android key",
                "default":false,
                "id":567,
                "link":"/api/v1/keys/android/567"
             }
        },
        "download":{},
        "title":"Hosted Application",
        "repo":"git@git.phonegap.com:alunny/36500_HostedApplication.git",
        "collaborators":[
            {
                "person":"andrew.lunny@nitobi.com",
                "role":"admin"
            }
        ],
        "role":"admin",
        "id":36500,
        "icon":{
            "filename":"null",
            "link":"/api/v1/apps/36500/icon"
        },
        "package":null,
        "version":null,
        "description":null,
        "debug":false,
        "private":true,
        "link":"/api/v1/apps/36500,
        "status":{
            "webos":"pending",
            "ios":"pending",
            "blackberry":"pending",
            "android":"pending",
            "symbian":"pending",
            "winphone":"pending"
        },
        "error":{},
        "phonegap_version":"1.3.0",
        "build_count":null
    }

### PUT https://build.phonegap.com/api/v1/apps/:id

Update an existing app - the contents of the app, the app's metadata, or both. The response will be a JSON representation of the app - the same as the `GET /api/v1/apps/:id` request.

Updating the metadata involves sending a JSON object as the parameter `data`. Available options in this JSON object are:

* __title__: the title of your application
* __package__: the app's package identifier (such as `com.phonegap.www`)
* __version__: the app's version (such as `0.0.1`)
* __description__: the app's description
* __debug__: whether your app will be built in [debug mode](/docs/phonegap-debug)
* __private__: whether the app has restricted visibility or not
* __phonegap_version__: which release of PhoneGap your app uses

Here is a simple example: updating an app's version:

<pre><strong>$ curl -u andrew.lunny@nitobi.com -X PUT -d 'data={"version":"0.2.0"}' https://build.phonegap.com/api/v1/apps/8</strong></pre>
    {
        "id":8,
        "version":"0.2.0",
        "keys":{
            "ios":null,
            "blackberry":null,
            "android":null
        },
        "repo":null,
        "download":{},
        "collaborators":[
            {
                "person":"andrew.lunny@nitobi.com",
                "role":"admin"
            }
        ],
        "title":"App From API",
        "role":"admin",
        "icon":{
            "filename":null,
            "link":"/api/v1/apps/8/icon"
        },
        "package":null,
        "link":"/api/v1/apps/8",
        "debug":false,
        "private":true,
        "description":null,
        "status":{
            "webos":"pending",
            "ios":null,
            "blackberry":"pending",
            "android":"pending",
            "symbian":"pending"
        },
        "error":{},
        "phonegap_version":"1.3.0",
        "build_count":12
    }

By default, the app will be built for all supported platforms once the metadata has been changed.

#### Signing Keys

As with creating a new app, you can specify a signing key to use for each platform that you wish to build for. Here is a sample post selecting a new Android key for an app:

<pre><strong>$ curl -u andrew.lunny@nitobi.com -X PUT -d 'data={"keys":{"android":457}}' https://build.phonegap.com/api/v1/apps/36500</strong></pre>
    {
        "keys":{
            "ios":{
                "title":"new iOS key",
                "default":false,
                "id":123,
                "link":"/api/v1/keys/ios/123"
             },
             "blackberry":null,
             "android":{
                "title":"changed android key",
                "default":false,
                "id":457,
                "link":"/api/v1/keys/android/457"
             }
        },
        "download":{},
        "title":"Hosted Application",
        "repo":"git@git.phonegap.com:alunny/36500_HostedApplication.git",
        "collaborators":[
            {
                "person":"andrew.lunny@nitobi.com",
                "role":"admin"
            }
        ],
        "role":"admin",
        "id":36500,
        "icon":{
            "filename":"null",
            "link":"/api/v1/apps/36500/icon"
        },
        "package":null,
        "version":null,
        "description":null,
        "debug":false,
        "private":true,
        "link":"/api/v1/apps/36500,
        "status":{
            "webos":"pending",
            "ios":"pending",
            "blackberry":"pending",
            "android":"pending",
            "symbian":"pending",
            "winphone":"pending"
        },
        "error":{},
        "phonegap_version":"1.3.0",
        "build_count":null
    }


#### Updating a file-based application

If the application has been created from a file upload, you can include a new `index.html`, zip file, or tar.gz file as the `file` parameter in your request to update the contents.

<pre><strong>$ curl -u andrew.lunny@nitobi.com -X PUT -F file=@/Users/alunny/new/index.html https://build.phonegap.com/api/v1/apps/8</strong></pre>

#### Updating a repo-based application

To update an application from a remote repository, simply add the `pull` field to your `data` hash, and set it to `true`:

<pre><strong>$ curl -u andrew.lunny@nitobi.com -X PUT -d 'data={"pull":"true"}' https://build.phonegap.com/api/v1/apps/8</strong></pre>

PhoneGap Build will then attempt to download the new code from your remote repository, and rebuild your app for all supported platforms.

### POST https://build.phonegap.com/api/v1/apps/:id/:icon

Sets an icon file for a given app. Send a `png` file as the `icon` parameter in your post.

If you want to have multiple icons for different resolutions, you should _not_ use this API method. Instead, include the different icon files in your application package and specify their use in your [config.xml file](/docs/config-xml).

The response will have a `201` created status, and the application will be queued for building.

<pre><strong>$ curl -u andrew.lunny@nitobi.com -f icon=@icon.png https://build.phonegap.com/api/v1/apps/8/icon</strong></pre>

### POST https://build.phonegap.com/api/v1/apps/:id/build

Queue new builds for a specified app. The older builds will be discarded, while new ones are queued.

The builds will use the most current app contents, as well as the selected signing keys. The response will have a `202` (accepted) status.

<pre><strong>$ curl -u andrew.lunny@nitobi.com -X POST -d '' https://build.phonegap.com.com/apps/12/build</strong></pre>

To choose which platforms to build, include those as a JSON encoded parameter in your post:

<pre><strong>$ curl -u andrew.lunny@nitobi.com -X POST -d 'data={"platforms":["android","webos"]}' https://build.phonegap.com.com/apps/12/build</strong></pre>

Once the builds are queued, you will want to watch the results of `GET /api/v1/apps/:id` to see when each platform's status changes from `pending` (to `complete` or `error`).

### POST https://build.phonegap.com/api/v1/apps/:id/build/:platform

A simpler URL for the case of building a single platform:

<pre><strong>$ curl -u andrew.lunny@nitobi.com -X POST -d '' https://build.phonegap.com.com/apps/12/build/android</strong></pre>

### POST https://build.phonegap.com/api/v1/apps/:id/collaborators

Add a collaborator to work with you on a given application. You must be the owner/admin of the app to do this.

#### Required parameters

* __email__: The email address of your new collaborator
* __role__: What level of access the new collaborator will have - either `tester` (read-only access) or `dev` (read and write access)

If the user is on the system, a `201` (created) HTTP status code is returned, which lets you know that the user can now access your app. If she is not registered, a `202` (accepted) status is returned, and the collaboration is listed as pending.

A JSON representation of the affected app is returned after the collaboration has been added.

<pre><strong>$ curl -u andrew.lunny@nitobi.com -d 'data={"email":"newguy@nitobi.com","role":"dev"}' https://build.phonegap.com.com/apps/12/collaborators</strong></pre>
    {
        "id":12,
        "title":"App With Collaborators",
        "collaborators":{
            "link":"/api/v1/apps/9/collaborators",
            "pending":[
                {
                    "person":"newguy@nitobi.com",
                    "role":"developer"
                },
                {
                    "person":"nobody@nitobi.com",
                    "role":"tester"
                }
            ],
            "active":[
                {
                    "person":"admin@nitobi.com",
                    "role":"admin",
                    "id":9,
                    "link":"/api/v1/apps/9/collaborators/9"
                },
                {
                    "person":"foo@bar.com",
                    "role":"developer",
                    "id":13,
                    "link":"/api/v1/apps/9/collaborators/13"
                }
            ]
        },
        "package":"app.with.collaborators",
        ...
    }

### PUT https://build.phonegap.com/api/v1/apps/:id/collaborators/:id

Allows you to change the role for a particular collaborator on PhoneGap Build, to `dev` or `tester`.

If you are not the owner of an app, you will receive a `401` unauthorized response. You cannot change the email of a collaborator at present; trying to do so will return a `400` status.

<pre><strong>$ curl -u andrew.lunny@nitobi.com -d 'data={"role":"tester"}' -X PUT https://build.phonegap.com.com/apps/12/collaborators/13</strong></pre>
    {
        "id":13,
        "person":"foo@bar.com",
        "role":"tester",
        "app":{
            "id":12,
            "download":{},
            "title":"Hosted thru API",
            "role":"admin",
            "icon":{
                "filename":null,
                "link":"/api/v1/apps/12/icon"
            },
            "version":null,
            "package":null,
            "description":null,
            "debug":null,
            "link":"/api/v1/apps/12",
            "status":{
              "ios":"pending",
              "webos":"pending",
              "blackberry":"pending",
              "android":"pending",
              "symbian":"pending",
              "winphone":"pending"
            },
            "phonegap_version":"1.3.0",
            "build_count":null,
            "error":{}
        },
        "link":"/api/v1/apps/12/collaborators/13"
    }

### POST https://build.phonegap.com/api/v1/keys/:platform

Add a signing key to your PhoneGap Build account. The `platform` parameter has to be specified in the URL, and different files are required depending on the platform you're targeting.

#### iOS Signing Keys

To build for iOS, we require:

* a `p12` certificate file
* a `mobileprovision` file
* the password to access your certificate
* a title for your certificate-profile pair

Details on how to obtain these files are in our [iOS Signing](/docs/ios-builds) documentation.

A sample post would look like this:

<pre><strong>$ curl -u andrew.lunny@nitobi.com -F cert=@My_Certificate.p12 -F profile=@MyDevices.mobileprovision -F 'data={"title":"Developer Cert","password":"12345678"}' https://build.phonegap.com/api/v1/keys/ios</strong></pre>
     {
        "title":"Developer Cert",
        "default":false,
        "id":11,
        "link":"/api/v1/keys/ios/11",
        "provision":"meandmyteam.mobileprovision",
        "cert_name":"My_Certificate.p12"
     }

#### Android Keys

To sign your Android builds, we require:

* a `keystore` file
* the alias used for that keystore
* your keystore password (`keystore_pw`)
* your private key password (`key_pw`)
* a title for your key

Details on how to get your keystore file and the associated data are available in our [Android Code Signing](/docs/android-signing) documentation.

A sample post would look like this:

<pre><strong>$ curl -u andrew.lunny@nitobi.com -F keystore=@android.keystore -F 'data={"title":"Android Key","alias":"release","key_pw":"90123456","keystore_pw":"78901234"}' https://build.phonegap.com/api/v1/keys/android</strong></pre>
    {
        "title":"Android Key",
        "default":false,
        "id":2,
        "alias":"release",
        "link":"/api/v1/keys/android/2"
    }

#### BlackBerry Keys

To sign your Blackberry builds, we require:

* a `sigtool.csk` file
* a `sigtool.db` file
* the password to your key
* a title for your key

How to obtain the `sigtool` files is outlined in our [BlackBerry Keys](/docs/blackberry-keys) documentation.

A sample post would look like this:

<pre><strong>$ curl -u andrew.lunny@nitobi.com -F db=@sigtool.db -F csk=@sigtool.csk -F 'data={"title":"My BB Key","password":"78901234"}' https://build.phonegap.com/api/v1/keys/blackberry</strong></pre>
    {
        "title":"My BB Key",
        "default":false,
        "id":2,
        "link":"/api/v1/keys/blackberry/2"
    }

### DELETE https://build.phonegap.com/api/v1/apps/:id

Delete your application from PhoneGap Build - will return either a `202` (accepted) status, or `404` (if the app cannot be found).

<pre><strong>$ curl -u andrew.lunny@nitobi.com -X DELETE https://build.phonegap.com/api/v1/apps/8</strong></pre>
    {
        "success":"app 8 deleted"
    }

### DELETE https://build.phonegap.com/api/v1/apps/:id/collaborators/:id

Remove a collaborator from a project that you own.

<pre><strong>$ curl -u andrew.lunny@nitobi.com -X DELETE https://build.phonegap.com.com/apps/12/collaborators/13</strong></pre>
    {
        "success":"foo@bar.com removed from app 9"
    }

### DELETE https://build.phonegap.com/api/v1/keys/:platform/:id

Delete a signing key from PhoneGap Build - will return either a `202` (accepted) status, or `404` (if the key cannot be found).

<pre><strong>$ curl -u andrew.lunny@nitobi.com -X DELETE https://build.phonegap.com/api/v1/keys/android/8</strong></pre>
    {
        "success":"android key 8 deleted"
    }
