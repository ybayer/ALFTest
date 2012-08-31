# Preparing Your Application for PhoneGap Build

PhoneGap Build requires an application to be packaged in a specific
manner that may not be intuitive at first.

We use an open packaging model that follows the [W3C Widget
Packaging specification](http://www.w3.org/TR/widgets/).

The following is a guide to help package your application for PhoneGap Build.

##Sections

1. [What Do I Upload?](#what_do_i_upload)
2. [How Do I Configure My Application?](#configure_application)
3. [What's Next?](#whats_next)
3. [Where can I Get Help?](#whats_next)


<a id="#what_do_i_upload"></a>
###What Do I Upload?

####Preparing the Assets

PhoneGap Build only requires the assets of your application. This is
essentially your www directory which contains your html, css, images,
js files, etc.

PhoneGap Build will most likely fail to compile your application
if native files are uploaded (.h, .m, .java, etc).

####Removing Unnecessary Files

Once you've included the necessary assets, remove the `phonegap.js`
(cordova.js) as Build will automatically inject it during compile
time.

####Why must you delete the `phonegap.js`?

PhoneGap requires a different JavaScript file for each platform and
using an incompatible `phonegap.js` will result in errors when
running your application.

####Making Sure You can Still Access the PhoneGap API

Once you've deleted the `phonegap.js` you'll need to make sure that your
application can still access the PhoneGap API.

To do so, simply ensure that the following reference is made in your `index.html`

    <script src="phonegap.js"></script>

<a id="#configure_application"></a>
###How Do I Configure My Application?

PhoneGap Build supports a configuration XML file, `config.xml`.

This configuration file allows you to modify things like the
application's title, icons, splash screens, and other properties.

For more information on the config.xml see our
[documentation](/docs/config-xml).

<a id="whats_next"></a>
###What's Next?

You should now be ready to proceed with building your application on
PhoneGap Build.

However, we also recommend reading the following documentation as it will help
achieve a better understanding of PhoneGap Build.

* [Start Compiling with PhoneGap Build](/docs/start).

* [Debugging Your Applications with PhoneGap Build](/docs/phonegap-debug)

<a id="help"></a>
###Where can I get help?

If you're running into errors during compilation we have prepared
a list of
[common errors and their solutions.](/docs/build-failed).

If your question has still not been answered, or you would like to
provide some feedback to our team we use our
[community support channel](http://community.phonegap.com)
for most of our communication. Don't hesitate to drop us a line!
