An attempt to minimize .cshtml files with gulp.
 

# Usage
```
var minifyCshtml = require('gulp-cshtml-minify'),
    gulp = require('gulp');

gulp.src("test.cshtml")
    .pipe(minifyCshtml())
    .pipe(gulp.dest("result"));
```
---

# Features
1. Handles `<pre>` and `<textarea>` blocks properly
2. Minifies inline `<script>` blocks with uglify-js
3. Minifies inline `<style>` blocks with clean-css

# Sample
### Input
```
@model Be.An.NS
@using Microsoft.AspNetCore.Http.Features

@{
    var consentFeature = Context.Features.Get<ITrackingConsentFeature>();
    var showBanner = !consentFeature?.CanTrack ?? false;
    var cookieString = consentFeature?.CreateConsentCookie();
}

<style>
    body{
        color:red;
        margin:0 15px 2.5rem 25px;
    }

    .card .header{
        float:left;
    }
</style>

@if (showBanner)
{
    <nav id="cookieConsent" class="navbar navbar-default navbar-fixed-top" role="alert">
        <div class="container">
            <div class="navbar-header">
                <button type="button" class="navbar-toggle" data-toggle="collapse" data-target="#cookieConsent .navbar-collapse">
                    <span class="sr-only">Toggle cookie consent banner</span>
                    <span class="icon-bar"></span>
                    <span class="icon-bar"></span>
                    <span class="icon-bar"></span>
                </button>
                <span class="navbar-brand"><span class="glyphicon glyphicon-info-sign" aria-hidden="true"></span></span>
            </div>
            <div class="collapse navbar-collapse">
                <p class="navbar-text">
                    Use this space to summarize your privacy and cookie use policy.
                </p>
                <div class="navbar-right">
                    <a asp-controller="Home" asp-action="Privacy" class="btn btn-info navbar-btn">Learn More</a>
                    <button type="button" class="btn btn-default navbar-btn" data-cookie-string="@cookieString">Accept</button>
                </div>
                <pre>
                        I should
                        not
                        be
                        minified
                </pre>
            </div>
        </div>
    </nav>
    <script>
        (function () {
            document.querySelector("#cookieConsent button[data-cookie-string]").addEventListener("click", function (el) {
                document.cookie = el.target.dataset.cookieString;
                document.querySelector("#cookieConsent").classList.add("hidden");
            }, false);

            var superLongName = 2;
            var verysuperlongname = 3;
            var total = superLongName + verysuperlongname;
            console.log(total);
        })();
    </script>
}
```

### Output
```
@model Be.An.NS
@using Microsoft.AspNetCore.Http.Features
@{var consentFeature = Context.Features.Get<ITrackingConsentFeature>();var showBanner = !consentFeature?.CanTrack ?? false;var cookieString = consentFeature?.CreateConsentCookie();}<style>body{color:red;margin:0 15px 2.5rem 25px;}.card .header{float:left;}</style>@if (showBanner){<nav id="cookieConsent" class="navbar navbar-default navbar-fixed-top" role="alert"><div class="container"><div class="navbar-header"><button type="button" class="navbar-toggle" data-toggle="collapse" data-target="#cookieConsent .navbar-collapse"><span class="sr-only">Toggle cookie consent banner</span><span class="icon-bar"></span><span class="icon-bar"></span><span class="icon-bar"></span></button><span class="navbar-brand"><span class="glyphicon glyphicon-info-sign" aria-hidden="true"></span></span></div><div class="collapse navbar-collapse"><p class="navbar-text">Use this space to summarize your privacy and cookie use policy.</p><div class="navbar-right"><a asp-controller="Home" asp-action="Privacy" class="btn btn-info navbar-btn">Learn More</a><button type="button" class="btn btn-default navbar-btn" data-cookie-string="@cookieString">Accept</button></div>
<pre>
                        I should
                        not
                        be
                        minified
                </pre>
</div></div></nav><script>!function(){document.querySelector("#cookieConsent button[data-cookie-string]").addEventListener("click",function(e){document.cookie=e.target.dataset.cookieString,document.querySelector("#cookieConsent").classList.add("hidden")},!1);console.log(5)}();</script>}
```