# ✨ Aurora Borealis

### Motivation

[Here ](https://github.com/clarkmaio/AuroraBorealis)you can find a little dashboard I wrote using **Streamlit**.

The main purpose of the dashboard is to provide an application to visualize **Kp** and **Ap** values.

I am not really an expert or a physician so that you can find some info about this values [here ](https://auroraforecast.is/kp-index/)and on [Wikipedia](https://en.wikipedia.org/wiki/K-index). In general just keep mind the following:

#### High Kp means strong Auora



I've always been fascinated by Aurora Borealis so that I've started asking my self if this is a phenomena that happens with regularity and if yes which are the pattern.

### Patterns

It is well known that Aurora has a yearly cycle and it is stronger on average during spring and autumn

![](.gitbook/assets/aurora\_monthly\_seasonality.PNG)

By a simple analysis it turns out that there is also a seasonality of about _**7 years**_. For example in 2022 we are in a ascending phase meaning that I expect on average stronger Aurora in the next years.

### The dashboard

Thank to the **Streamlit** you can easily play with the dashboard using the link:

{% embed url="https://share.streamlit.io/clarkmaio/auroraborealis/main/dashboard.py" %}
AuroraBoralis dashboard link
{% endembed %}

**Note**: if it has been a while since last time dashboard has been used it could take a couple of minutes to "wake" it... just be patient.

You can choose the timeseries granularity with the _**Resolution**_ input. You can also download the whole dataset using the _**Download**_ button.

![](.gitbook/assets/aurora\_dashboard.PNG)

Every time the dashboard is launched the dataset is downloaded from [GFZ Postam website](https://www-app3.gfz-potsdam.de/kp\_index/Kp\_ap\_since\_1932.txt) so that you will always have updated data.



In the repository you will find the function to easily download data on your own:

```
import matplotlib.pyplot as plt
from AuroraBorealis.data_scraper import DataScraper

data_scraper = DataScraper()
data = data_scraper.load_data()

plt.plot(data, subplots=True)
```



### What's next?

* Introduction of moon phase as variable&#x20;
* Deeper seasonality analysis
* Autoregressive model for short term prediction

Stay tuned!

Ciao



