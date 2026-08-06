# Silverpeas Looks

The graphical looks of [Silverpeas](https://www.silverpeas.org), the collaborative and social-network platform.

A *look* defines the whole user experience of a Silverpeas platform: the banner, the navigation menu, the platform
home page and the home page of each collaborative space. It is not a standalone application but a set of JSP pages,
JSP tag files, CSS/JavaScript assets and configuration files that Silverpeas Core loads at runtime to render the
user's workspace.

This project delivers the **Aurora** look, the default look of Silverpeas, along with four interchangeable skins:
*Aurora*, *Sobre*, *Prima* and *Waves*. All of them share the same pages and behaviour; they differ by their
stylesheet and by the values of their settings.

## Requirements

* Java 17
* Maven 3
* A Silverpeas runtime (WildFly) to deploy to — the look depends on Silverpeas Core and on several Silverpeas
  components (news, delegated news, almanach, question/reply, RSS aggregator, gallery, blog, web pages), all
  provided by the platform at runtime.

## Building

```bash
mvn clean install
```

The build produces two artifacts:

| Module                 | Packaging | Content                                                                        |
|------------------------|-----------|--------------------------------------------------------------------------------|
| `aurora-war`           | war       | the look itself: Java helpers, JSP pages and tag files, CSS and JavaScript      |
| `aurora-configuration` | jar       | what is installed into the Silverpeas configuration: settings, i18n bundles, XML publication templates, and the skins' CSS and images served from `/weblib` |

Both modules minify their JavaScript and CSS at build time. Use the `skipMinify` profile to skip that step while
iterating locally:

```bash
mvn clean install -PskipMinify
```

A [development container](.devcontainer) based on the `silverpeas/silverdev` image is provided; it comes with the
required JDK and Maven and mounts your local Maven repository, SSH keys and Git configuration.

## Configuring a look

Everything a look offers is driven by its settings bundle, one properties file per skin, located in
`aurora/aurora-configuration/src/main/config/properties/org/silverpeas/util/viewGenerator/settings/`:

* `lookSettings.properties` declares the looks available on the platform; its `Initial` entry names the default one.
* `Aurora.properties`, `Sobre.properties`, `prima.properties` and `waves.properties` hold the settings of each skin:
  banner content and spaces, news sources and their rendering, next events, latest publications, search form,
  shortcuts, weather, directory, space home pages, and so on. Each key is documented in the file itself.

Two features have their own configuration files, next to the ones above:

* **Weather** (`org/silverpeas/weather/settings/weather.properties`) — the home page can display a weather forecast
  for a list of cities, fetched from OpenWeatherMap, AccuWeather or Yahoo Weather. Disabled until a service and an
  API key are set.
* **Matomo** (`org/silverpeas/statistics/settings/matomo.properties`) — optional injection of a
  [Matomo](https://matomo.org) analytics tracking script into the rendered pages. Disabled by default.

Beyond the settings, the home page of a given space can be configured by the space administrators themselves,
through a *web pages* application bound to the `auroraspacehomepage` XML template shipped by this project. What the
template defines takes precedence over the platform-wide settings for that space.

## Contributing

Bugs and feature requests are tracked on the [Silverpeas bug tracker](https://tracker.silverpeas.org). Pull
requests are welcome; their title has to start with `Bug #<n>`, `Feature #<n>` or `Support #<n>`, referring to the
tracked issue, as the continuous integration relies on it.

## License

Silverpeas Looks is released under the [GNU Affero General Public License v3](license.txt), with the
[Silverpeas FLOSS exception](exceptions.txt).
