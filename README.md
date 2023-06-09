# Strava Heatmap String Builder

* [Summary](#Summary)
* [How to Use](#How-to-Use)
* [Examples](#Examples)
* [Some Final Considerations](#Some-Final-Considerations)

## Summary

Strava's Heatmap provides very useful data for discovering unofficial trails & common backcountry routes. This data can be imported as a "Custom Layer" in [Caltopo](https://caltopo.com/). However, doing so requires building a URL string containing configuration & authentication data. This tool assists in the creation of this string.

*This tool is intended for Caltopo strings. For use with OpenStreetMap, you may wish to pass `-t -o`*.

| Before | After |
| ------ | ----- |
| ![Before](https://raw.githubusercontent.com/zimmertr/StravaHeatmapStringBuilder/main/screenshots/before.png?raw=true "Before") | ![After](https://raw.githubusercontent.com/zimmertr/StravaHeatmapStringBuilder/main/screenshots/after.png?raw=true "After") |


## How to Use

1. [Create](https://www.strava.com/register/free) a Strava account. Strava requires an account to access the Heatmap data.

2. Build a URL string using Python: `python main.py`. You will be prompted for your Strava credentials. Additional optional arguments are documented in the table below:
   | Argument | Description                                            | Allowed Values                       | Default |
   | -------- | ------------------------------------------------------ | ------------------------------------ | ------- |
   | -s       | The Heatmap Server to Use (Ignored if also using `-o`) | `[a, b, c]`                          | `a`     |
   | -a       | The activity type to show on the Strava Heatmap data   | `[run, ride, winter, water, all]`    | `all`   |
   | -c       | The color to use for the Strava Heatmap data           | `[blue, bluered, purple, hot, gray]` | `hot`   |
   | -r       | The tile resolution to use for the Strava Heatmap data | *Any integer value*                  | `256`   |
   | -t       | Include a `tms:` prefix to the URL                     | `N/A`                                | `False` |
   | -o       | Use OSM Format for URL                                 | `N/A`                                | `False` |
   
3. [Create](https://caltopo.com/account/signup) a Caltopo account and sign in.

4. Add a [Custom Map Layer](https://blog.caltopo.com/2014/04/25/custom-map-layers/) in Caltopo using the following properties:

   | Property     | Value                       |
   | ------------ | --------------------------- |
   | Type         | `Tile`                      |
   | Name         | `Strava Heatmap`            |
   | URL Template | *URL Produced Above*        |
   | Max Zoom     | `16`                        |
   | Overlay?     | `Yes - Transparent Overlay` |

5. Click "Save To Account" and refresh the webpage.

6. Check "Strava Heatmap" in "Your Layers" on the Map Layers sidepanel to activate the Heatmap overlay.

## Examples

Using Python & the default configuration:
```bash
$> python main.py
Enter your Strava Email Address: xqsaxgrnpbffgslmge@tpwlb.com
Enter your Strava Password:

Your heatmap URL is:

https://heatmap-external-a.strava.com/tiles-auth/all/hot/{z}/{x}/{y}.png?px=256&Key-Pair-Id=APKAIDPUN4QMG7VUQPSA&Policy=eyJTdGF0ZW1lbnQiOiBbeyJSZXNvdXJjZSI6Imh0dHBzOi8vaGVhdG1hcC1leHRlcm5hbC0qLnN0cmF2YS5jb20vKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MTMzNTA4OX0sIkRhdGVHcmVhdGVyVGhhbiI6eyJBV1M6RXBvY2hUaW1lIjoxNjgwMTExMDg5fX19XX0_&Signature=eyeaRSIwev0ev1xV7eNMX-vnKdrpcV4FDhakfhBt6tNQdKOLilyVU6ngvOvur5VMxuXGir~ogvDdjZtCuyI-rWrwu2REVyj7vKLN5v5e5WcBK8XPaLr4dOHhlvfzZJvKw3AG9w0EgIFszKHZuBHbwA6Sl9dTO5NarOaMtZnVIvpGqRnZxGoBlGQROs-qsUFO9cjkRxWK-sgadRBGnH8vR9WTGcvO-mbdzKKfCMb9j8TTOzFyAbUEZJDHtkHYi-y9KHEhQtL9ZvwLu-xpX0rEgAcjfrO3CoaNaAmOdqhgedK5uWK42Y15ozRqsgEt~c2VzqnYZW4mljhO7339IYNtPw__

This URL will expire on April 9.
```

Using Docker & optional configuration arguments:
```bash
$> docker run -it $(docker build -q .) -c blue -a winter -t -o
Enter your Strava Email Address: xqsaxgrnpbffgslmge@tpwlb.com
Enter your Strava Password:

Your Strava Heatmap URL is:

tms:https://heatmap-external-{switch:a,b,c}.strava.com/tiles-auth/winter/blue/{zoom}/{x}/{y}.png?px=256&Key-Pair-Id=APKAIDPUN4QMG7VUQPSA&Policy=eyJTdGF0ZW1lbnQiOiBbeyJSZXNvdXJjZSI6Imh0dHBzOi8vaGVhdG1hcC1leHRlcm5hbC0qLnN0cmF2YS5jb20vKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MTMzNTExOH0sIkRhdGVHcmVhdGVyVGhhbiI6eyJBV1M6RXBvY2hUaW1lIjoxNjgwMTExMTE4fX19XX0_&Signature=oYuTN2g0hiv4Aoy4kbIgkyhQ36kiuIxY~ParaaqQcXZwOngySj8YQGrFjX480R83Iwqi-vgenTX8uSS9FUenpd-PSKhgwlU6ShrD3ya6P5~7re1zjLiaUR6doJ5mqVm1EK8hNU0XT~QfYLQ0RhIbuNjQL0kumqjOJA3-Bq5MJ9zRhMr~9uy7JRkOCFmCFkqfmCzaDfgJahrVuoe2tNTghm1dxyA5bfmucoSU0dK3rgq0pQ0XuNw9o4R-YeiSc7GMPO9hSvaXrj2RIdmCo8Ot6GfpdaDoiJ7DxMtT3WhjL6I4IFVmf6PRv7mD~c6VPGVOMYB6IimM1wYAnhRaR5txuA__

This URL will expire on April 9.
```

## Some Final Considerations

1. Strava expires its authentication cookies after approximately one week. If your Heatmap layer stops appearing when you check the box, you will need to regenerate your authentication cookies and build a new URL. To update the URL in Caltopo, simply navigate to `Add -> Custom Source` again and click `Load From`. Select your previous Strava Heatmap layer, update the `URL Template` data with the new URL, and give it a new `Name` to differentiate it from the old one. Finally, click `Save To Account` and refresh the page. You will now have two available layers, only one of which will work. To delete the old one, click `Your Data` on the top right of Caltopo, navigate to `Your Layers`, and delete the old one by clicking the red `X`. Then refresh the webpage to reload the available layers. 

2. Strava will disable your account if you attempt to authenticate too many times in a period of time. Be careful not to run this script repeatedly!

3. **DO NOT SHARE** your authentication cookie data with anyone. Using this information, someone can impersonate you and potentially do things like post as you or even delete your account! 

4. This uses the [stravacookies](https://pypi.org/project/stravacookies/) Python package by [solitone](https://github.com/solitone/stravacookies) for retrieving the necessary cookies to provide authenticated access to the Strava Heatmap data.
