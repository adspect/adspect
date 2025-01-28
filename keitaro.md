# Integration with Keitaro

Adspect supports native integration with Keitaro tracker using a custom filter.  It works as follows: you create separate flows
for money and safe pages in your Keitaro campaign.  The safe page flow is linked to an Adspect stream via a custom Adspect
filter, which uses stream settings to filter clicks and route invalid ones into that safe page flow. All other visitors not flagged
by the filter will fall through into the money page flow.

## Filter Installation

In order to use the Adspect filter you must upload its file to your Keitaro tracker's server.  Do the following:

1.  [Download the AdspectFilter.php file](https://clients.adspect.io/static/dist/keitaro/AdspectFilter.php);
2.  Upload the AdspectFilter.php file to the `/var/www/keitaro/application/filters` directory on the server where your Keitaro
    tracker is installed.

:::{note}
Filter installation should be done just once for a particular Keitaro server, or after reinstalling Keitaro tracker.
:::

## Campaign Setup

We recommend creating a separate Adspect stream for each Keitaro campaign.

1.  Create a stream in your Adspect clients area.  Money and safe page settings are irrelevant to Keitaro integration and may
    be skipped; only filtering-related stream settings are used.

2.  Create a separate flow for the safe page in your Keitaro campaign and put it above the money page flow:
    ```{image} _static/keitaro/flows.png
    :alt: Keitaro flows layout example
    ```

3.  In the safe page flow settings, on the Filters tab, add the "adspect" filter and enter your stream ID in
    the Adspect Stream ID field:
    ```{image} _static/keitaro/filter.png
    :alt: Keitaro flow filter setup example
    ```

4.  Direct traffic to your Keitaro campaign URL.

:::{tip}
In a Keitaro campaign, you'd normally put your money page in a default flow and safe page in a regular flow with Adspect filter.
However, you can set it up vice versa, i.e. with safe page in a default flow and money page in a regular flow with Adspect filter.
To do that, just change the mode of the Adspect filter in your money page flow from "IS" to "IS&nbsp;NOT."
:::
