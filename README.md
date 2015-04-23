# jsSocials - Simple Social Sharing

[![Build Status](https://travis-ci.org/tabalinas/js-socials.svg?branch=master)](https://travis-ci.org/tabalinas/js-socials)

Project site [js-socials.com](http://js-socials.com/)

**jsSocials** is a simple social network sharing jQuery plugin. It's flexible and easily extensible. 
Configure visual appearance. Choose one of several themes provided. Add any yet unsupported social network if needed.


## Demos

Find demos on the [project site](http://js-socials.com/demos/).


## Getting Started

1. Download the package
2. Add links to `jssocials.css` and chosen theme, e.g. `jssocials-theme-flat.css`
3. Add link to `font-awesome.css` (it's used for social network logos by default, yet you can replace it with image logo or other font if necessary)
4. Add link to `jquery.js` and plugin script `jssocials.min.js`
5. Apply jsSocials to the element on the page `$("#share").jsSocials({ shares: ["twitter"] })`

```html
<!DOCTYPE html>
<html>
<head>
    <link rel="stylesheet" type="text/css" href="font-awesome.css" />
    <link rel="stylesheet" type="text/css" href="jssocials.css" />
    <link rel="stylesheet" type="text/css" href="jssocials-theme-flat.css" />
</head>
<body>
    <div id="share"></div>

    <script src="jquery.js"></script>
    <script src="jssocials.min.js"></script>
    <script>
        $("#share").jsSocials({
            shares: ["twitter", "facebook", "googleplus", "linkedin", "pinterest"]
        });
    </script>
</body>
</html>
```

## Documentation

* [Themes](#themes)
* [Configuration](#configuration)
* [Methods](#methods)
* [Share](#share)
* [Build-in Shares](#build-in-shares)
* [Custom Share](#custom-share)
* [Adaptiveness](#adaptiveness)


### Themes

To turn on specific theme just link one of available stylesheets

* **jssocials-theme-flat.css** - flat theme
* **jssocials-theme-classic.css** - classical theme with raised buttons
* **jssocials-theme-minima.css** - minimalistic theme with logos instead of buttons
* **jssocials-theme-plain.css** - monochromatic theme

#### flat

![jsSocials - flat theme](http://gdurl.com/OI28)

#### classic

![jsSocials - classic theme](http://gdurl.com/gGQV)

#### minima

![jsSocials - minima theme](http://gdurl.com/ULek)

#### plain

![jsSocials - plain theme](http://gdurl.com/9OmN)


### Configuration

The config object may contain following options:
 
```javascript
{
    shares: ["twitter", "facebook", "googleplus", "linkedin", "pinterest"],
    url: "http://url.to.share",
    text: "text to share",
    showLabel: true,
    showCount: true
}
```

#### shares :`Array`

An array of shares. 

Each share can be

* string - name of share from registry `jsSocials.shares` (e.g. `"twitter"`)
* config - plain object with `share` as name and custom parameters specific for each share. Read more about share config in [Shares section](#shares).

For instance for twitter the config may look like:

```javascript
{
    share: "twitter",           // name of share
    label: "Tweet",             // share button text (optional)
    via: "artem_tabalin",       // custom twitter sharing param 'via' (optional)
    hashtags: "jquery,plugin"   // custom twitter sharing param 'hashtags' (optional)
}
```

#### url :`String`

A string specifying url to share. **window.location.href** used by default.

#### text :`String`

A string specifying text to share. The content of `<meta name="description">` or `<title>` used by default.

#### showLabel :`true|false|function(screenWidth)`

A boolean specifying whether to show the text on the share button. 
Also accepts function returning true|false depending on screen width for adaptive rendering. Read more in [Adaptiveness section](#adaptiveness)

#### showCount :`true|false|"inside"|function(screenWidth)`

A boolean or "inside" specifying whether and how to show share count. 
Also accepts function returning true|false|"inside" depending on screen width for adaptive rendering. Read more in [Adaptiveness section](#adaptiveness)


### Methods

#### destroy()

Destroys the shares control and brings the Node to its initial state.

````javascript

$("#share").jsSocials("destroy");

````

### option(key, [value])

Gets or sets the value of an option.
 
**key** is the name of the option.

**value** is the new option value to set. 

If `value` is not specified, then the value of the option `key` will be returned.

````javascript

// turn off share count
$("#share").jsSocials("option", "showCount", false);

// get sharing text
var text = $("#share").jsSocials("option", "text");

````

#### refresh()

Refreshes sharing control. 

```javascript

$("#share").jsSocials("refresh");

```


### Share

A share config has few general for all shares parameters. Yet each share can have specific parameters.
 
```javascript 

{
    share: "twitter",
    label: "Tweet",
    logo: "fa fa-twitter",
    renderer: function() { ... }
}

```

#### share :`String`

A string name of the share.
jsSocials supports following build-in shares: `"twitter" | "facebook" | "googleplus" | "linkedin" | "pinterest"`
However adding any new share is simple and described in [Custom Share section](#custom-share).

#### label :`String`

A string specifying the text to show on share button.

#### logo :`String`

A string specifying the share logo. 
It accepts following values:

* **css class** - any non-url string is rendered as `<i class="css class"></i>`. Font awesome is used by default, but it can be redefined with any other css class. 
* **image url** - string of image url format is rendered as `<img src="image url" />`.
* **image base64 url** - string of image base64 url format is rendered as `<img src="image base64 url" />`.

#### renderer :`function()`

A function returning `&lt;div&gt;` with custom share content. 
The `renderer` is used for custom share scenario, e.g. using standard sharing component for particular network. 
If `renderer` is specified, then all other share parameters are ignored.

This is how render native google plus share button with `renderer`:

```javascript

$("#share").jsSocials({
    shares: [{
        renderer: function() {
            var $result = $("<div>");
    
            var script = document.createElement("script");
            script.src = "https://apis.google.com/js/platform.js";
            $result.append(script);
    
            $("<div>").addClass("g-plus")
                .attr({
                    "data-action": "share",
                    "data-annotation": "bubble"
                })
                .appendTo($result);
    
            return $result;
        }
    }]
});

```

### Build-in Shares

The build-in socials shares have following configuration

#### twitter

```javascript
{
    label: "Tweet",
    logo: "fa fa-twitter",
    via: "",                // a Twitter username specifying the source of a Tweet.
    hashtags: ""            // a comma-separated list of hashtags to be appended Tweet text.
}
```

#### facebook

```javascript
{
    label: "Like",
    logo: "fa fa-facebook"
}
```

#### googleplus

```javascript
{
    label: "+1",
    logo: "fa fa-google-plus",
}
```

#### linkedin

```javascript
{
    label: "Share",
    logo: "fa fa-linkedin",
}
```

#### pinterest

```javascript
{
    label: "Pin it",
    logo: "fa fa-pinterest",
    media: ""               // a url of media to share
}
```


### Custom Share

To register custom share add it to `jsSocials.shares` registry.
This is how defined twitter share:

```javascript

jsSocials.shares.twitter = {
    label: "Tweet",
    logo: "fa fa-twitter",
    shareUrl: "https://twitter.com/share?url={url}&text={text}&via={via}&hashtags={hashtags}",
    countUrl: "https://cdn.api.twitter.com/1/urls/count.json?url={url}&callback=?",
    getCount: function(data) {
        return data.count;
    }
};

```

Each share contains following parameters:

#### label :`String`

The default value of share label to display on the share button.

#### logo :`String`

A default value of share logo. Accepts css class or image url.

#### shareUrl :`String|function()`

A string or a function returning a string specifying the sharing url.
It can contain any parameters in curly braces `{param}`. This parameter will be taken from share config. 
The `{url}` and `{text}` params are taken from jsSocials config.

#### countUrl :`String|function()`

A string or a function returning a string specifying the url to request shares count.
It can contain any parameters in curly braces `{param}`. This parameter will be taken from share config. 
The `{url}` param is taken from jsSocials config.
This parameter should be specified only if you going to show share counts.

#### getCount :`function(data)`

A function retrieving count value from response received from countUrl.
Accepts response data. The response `data` is used as count if function is not specified.
If `getCount` returns a number, it will be formatted (e.g. 1200 to 1.2K). 
Return a string value to avoid automatic formatting.


## License

MIT © Artem Tabalin
