---
title: "HEPA Filters for Resin 3D Printing?"
date: 2024-01-17T22:44:19-06:00
draft: false
---

I've been getting back into 3d printing again recently, and having a great time with it. 
I do resin printing, and anyone who does resin printing will tell you to be careful with your safety.
The stuff is toxic, after all.

Now, I do my best to be safe with resin. Gloves, dedicated work surfaces, dedicated trash can, IPA wash bottle on hand at all times, everything gets a blast of UV if there's any concern that something has become unintentionally contaminated, ect.
There is one thing that (until today) I haven't done a great job about: the fumes.

See, resin stinks.
As in, it smells bad.
It turns out, however, that it's more than just smelly, the fumes are toxic in their own, special ways. 
That raises the question: How do you deal with that?

## The Impetus
While trolling the Discord server of my favorite resin-3d-printing-youtuber, [Once in a Six Side](https://www.youtube.com/@OnceinaSixSide), I saw someone had posted asking about air filters.

![The query that started it all](/images/hepa_for_resin/image.png)

_The super ventilation system they're referencing is a pretty clever system Once in a Six Side put together in a [video](https://www.youtube.com/watch?v=zk9KcGAH_Zc)._

This got me thinking: I'm currently using ~~prayers to Nurgle~~ nothing to keep me from inhaling resin fumes most of the time, so what _should_ I be doing?

## Phase One: How big is resin, anyways?
So, obviously, I need some kind of filter. I have a 3m filter mask I use sometimes, but I also use this room for painting, so something that doesn't have to me strapped to my face to help is ideal. So, what kind of filter do I need? Well, to answer that I need to know what's in the air to grab.

First thought: surely that's in the MSDS, right? Wrong. 
Most vendors only seem to give `Form: Liquid` and `Color: Grey`, leaving all the other physical characteristics in their MSDS null.

![Anycubic MSDS](/images/hepa_for_resin/anycubic_msds.png)

After a few DuckDuckGo searches, I eventually found this article: Particle and vapor emissions from vat polymerization desktop-scale 3-dimensional printers[^1]
What do you know, that's exactly what I'm after!

After reading through most of the paper, here's the bit we're after:

>During printing, particle emission yields (#/g printed) ranged from 1.3 ± 0.3 to 2.8 ± 2.6 × 108 (SLA printers) and from 3.3 ± 1.5 to 9.2 ± 3.0 × 108 (DLP printers). Yields for number of particles with sizes 5.6 to 560 nm (#/g printed) were 0.8 ± 0.1 to 2.1 ± 0.9 × 1010 and from 1.1 ± 0.3 to 4.0 ± 1.2 × 1010 for SLA and DLP printers, respectively. TVOC yield values (μg/g printed) ranged from 161 ± 47 to 322 ± 229 (SLA printers) and from 1281 ± 313 to 1931 ± 234 (DLP printers). Geometric mean mobility particle sizes were 41.1–45.1 nm for SLA printers and 15.3–28.8 nm for DLP printers.

Okay, cool. We know that we need to get a filter that can catch things in the single-digit nanometer range. So... what filter can do that?

## Phase two: No, I don't want to buy your cold war surplus gas mask
Let's go back to that MSDS. Here's their official recommendations for respiratory protection:

>In case of insufficient ventilation, wear suitable respiratory equipment. Half-
face mask (DIN EN 140). full face mask (DIN EN 136). Filter type: (ABEK)
(EN14387). The filter class must be suitable for the maximum contaminant
concentration (gas/vapour/aerosol/particulates) that may arise when handling
the product. If the concentration is exceeded, self-contained breathing
apparatus must be used. (EN 137) [^2]

Okay... I have no idea what those numbers mean. Surely `filter type: ABEK` is a searchable term.

![ABEK](/images/hepa_for_resin/ABEK.png)

Yeah, shoulda seen that one coming. Searching the other identifiers gave similar levels of not-what-I'm-looking-for, so back to the drawing board.

## Phase three: Wait, will that old air purifier in storage work?
Backstory: I have an old "air purifier" in storage. Backstory over. 

_(Okay, I bought it in college to try and help with the dust from the decaying house I lived in. It didn't really help)_

What kinda filter does _it_ have? According to the [Amazon listing](https://www.amazon.com/Purifier-Allergies-Filter-Bedroom-minators/dp/B088FHCS83/ref=sr_1_3_pp?crid=2UPZDMTZ1Y3U3&qid=1705556769&sprefix=air+purifier+,aps,151&ufe=app_do:amzn1.fos.006c50ae-5d4c-4777-9bc0-4513d670b6bc&th=1), it's a HEPA filter.
I wonder, will HEPA catch our single-digit nanometer toxins?

According to the [EPA](https://www.epa.gov/indoor-air-quality-iaq/what-hepa-filter)[^3], who knows! Okay, technically they say:

>The diameter specification of 0.3 microns corresponds to the worst case; the most penetrating particle size (MPPS). Particles that are larger or smaller are trapped with even higher efficiency. Using the worst case particle size results in the worst case efficiency rating (i.e. 99.97% or better for all particle sizes).

But, is there a lower limit to that? 
Are our 3nm particles (0.003 microns) so small that they end up sneaking through? 
Is there someone who's done, say, a specific scientific study on exactly that?
Turns out, everyone's favorite `.gov` domain has!
(And no, I'm not referring to ylwd.gov.)
Once again, NASA is here to save us!

Continuing the trend of scientists naming things in the most boring way possible, enter "Submicron and Nanoparticulate Matter Removal by HEPA-Rated Media Filters and Packed Beds of Granular Materials"[^4]. It's an interesting read, but here's the bit we care about (from the conclusion):

>The phenomena associated with particulate matter removal by HEPA media filters and
packed beds of granular material have been reviewed relative to their efficacy for removing fine
(<2.5 μm) and ultrafine (<0.01 μm) sized particulate matter. The ultrafine particulate range consti-
tutes the range defined as nanoparticulates. [...] HEPA-rated media provides superior performance for removing virtually 100% of particulates.

Amazing. HEPA filters can remove 100% of nanoparticulates. We did it Reddit.

# Conclusion
Based on the two studied cited, HEPA filters can remove "virtually 100%" of the particles of the size that resin printing emits.
Based on this, it would suggest that using a HEPA rated filter in an air purifier could be an effective way to minimize the risks associated with resin fumes as a pervasive threat.

Personally, I've pulled my old air purifier out of storage, and ordered a new filter for it. 
I'll continue to use the half-mask when doing large jobs where there is a spike in pollutants, but it's good to know that keeping an air purifier running in the room can help keep the risks from long-term exposure to a minimum.



[^1]: Stefaniak, A B et al. “Particle and vapor emissions from vat polymerization desktop-scale 3-dimensional printers.” Journal of occupational and environmental hygiene vol. 16,8 (2019): 519-531. doi:10.1080/15459624.2019.1612068

[^2]: Anycubic resin MSDS. https://cdn.shopify.com/s/files/1/0245/5519/2380/files/EN_SDS_3D_Printing_UV_Sensitive_Resin___SDS_SGS_GHS_ANNEXII_CLP_2021616_SGS-SHENZHEN-03_Shenzhen_Anycubic_Technology_Co._Ltd.pdf?v=1660036934

[^3]: EPA.gov,  What is a HEPA filter? https://www.epa.gov/indoor-air-quality-iaq/what-hepa-filter

[^4]: NASA, https://ntrs.nasa.gov/api/citations/20170005166/downloads/20170005166.pdf