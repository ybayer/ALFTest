# PhoneGap Plugins

Adobe® PhoneGap™ Build supports a curated selection of PhoneGap Plugins, to extend the native functionality exposed by the PhoneGap native-app container.

Plugins need to implemented differently for each platform, and may not be supported across all PhoneGap platforms. If you're deploying across multiple platforms, be sure that the experience degrades gracefully for users who do not have the plugin available.

## Including a plugin in your project

There are two steps to including a plugin in your project: referencing the JavaScript code for the plugin, and importing the native code.

### Referencing the JavaScript code

As with `phonegap.js`, the JavaScript code for a plugin is inserted into your project by PhoneGap Build at build time. Plugins will usually depend on PhoneGap being loaded already, so insert a script tag after the `phonegap.js` tag, like so:

    <script src="phonegap.js"></script>
    <script src="childbrowser.js"></script>

### Importing the native code

To import the native code into your PhoneGap Build project, you will need to
add the correct `<gap:plugin>` tag to your [config.xml](config-xml) file. Here
is the tag for an Example plugin:

    <gap:plugin name="Example" />

You can specify a version of a plugin to run with the `version` attribute -
this is optional.

    <gap:plugin name="Example" version="2.2.1" />

You can also use the tilde (~) operator to specify _fuzzy_ versions - this will
ensure that you have the latest version of a plugin with the same major version.
For example, you could replace the tag above with:

    <gap:plugin name="Example" version="~2" />

which would load the latest 2.x version, but not anything with a different
major/initial version number. This version tag:

    <gap:plugin name="Example" version="~2.2" />

would load the latest 2.x version so long as x is greater or equal to 2. This
version tag:

    <gap:plugin name="Example" version="~2.2.3" />

would load the latest 2.2.x version so long as x is greater or equal to 3.

Plugins may require configuration information to be present; this can be done
with adding `<param>` children to the `<gap:plugin>` tag:

    <gap:plugin name="ExampleAPI">
        <param name="APIKey" value="12345678" />
        <param name="APISecret" value="12345678" />
    </gap:plugin>

## Supported Plugins

### ChildBrowser

* versions available: `1.0.2`, `2.0.1`, `2.1.0`, `3.0.1`, `3.0.4`
* supported platforms: `iOS`, `Android`

The ChildBrowser plugin allows you to display external web pages within your
app, in a subview. It is preferable to a regular link in that it allows the
user to press a button to dismiss that view, returning control back to your app.

__Important__: If your app targets PhoneGap `1.4.1` or below, use the
ChildBrowser version `1.0.2`. If your app targets PhoneGap/Cordova `1.5.0` to
`1.8.1`, use `2.1.0`. If your app targets PhoneGap/Cordova `1.9.0` or above,
please use `3.0.4`.

To include the ChildBrowser plugin, add the `<script>` tag to your `index.html` page:

    <script src="phonegap.js"></script>
    <script src="childbrowser.js"></script>

And add the appropriate `gap:plugin` tag:

    <gap:plugin name="ChildBrowser" /> <!-- latest release -->
    <gap:plugin name="ChildBrowser" version="~3" /> <!-- for Cordova (1.9.0+) -->
    <gap:plugin name="ChildBrowser" version="~2" /> <!-- for Cordova 1.5.0 - 1.8.1 -->
    <gap:plugin name="ChildBrowser" version="~1" /> <!-- for PhoneGap <= 1.4.1 -->

For legacy reasons, the `feature` tag is still supported for the ChildBrowser;
this can be done as so (with an absolute version number):

    <feature name="http://plugins.phonegap.com/ChildBrowser/2.0.1" />
    <!-- or -->
    <feature name="http://plugins.phonegap.com/ChildBrowser/1.0.2" />

The full JavaScript API is available on [the plugin's README](https://github.com/alunny/ChildBrowser/blob/master/README.md).

If you're using the 1.0 release, please [the older docs](https://github.com/alunny/ChildBrowser/blob/one-x/README.md).

## Contributing Plugins

PhoneGap Plugins can be made compatible with PhoneGap Build with the use of a `plugin.xml` file, a README for software to use.

Right now, you cannot submit your own plugins to PhoneGap Build and have them
included on our system. We are working on the infrastructure changes to allow
this support.

This format is not rigorously specified yet - please see the [ChildBrowser's XML file](https://github.com/alunny/ChildBrowser/blob/master/plugin.xml) for an example. To test your `plugin.xml` file locally, you can use [our `pluginstall` script](https://github.com/alunny/pluginstall) - if the plugin can be installed using that script, we can probably run it on PhoneGap Build.

If you're interested in contributing plugins, or contributing to the plugin specification and tooling, please [contact us on Twitter](http://twitter.com/PhoneGapBuild).
