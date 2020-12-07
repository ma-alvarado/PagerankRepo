
In this project for my data mining course, I created a simple search engine for the website https://www.lawfareblog.com .
This website provides legal analysis on US national security issues.

**Updated 12/6/20**



## The Power Method

**Part 1:**
Checking to ensure that my implementation is working,

```
$ python3 pagerank.py --data=./small.csv.gz --verbose
DEBUG:root:computing indices
DEBUG:root:computing values
INFO:root:rank=0 pagerank=6.0257e-01 url=4
INFO:root:rank=1 pagerank=4.6414e-01 url=6
INFO:root:rank=2 pagerank=3.4544e-01 url=5
INFO:root:rank=3 pagerank=1.9498e-01 url=2
INFO:root:rank=4 pagerank=9.9210e-02 url=3
INFO:root:rank=5 pagerank=8.9347e-02 url=1
```

**Part 2:**
Using the `--search_query` feature, which takes a string as a parameter, the program returns all urls that match the query string sorted, according to their pagerank.
Essentially, this gives the most important pages on the blog related to the query.

```
$ python3 pagerank.py --data=./lawfareblog.csv.gz --search_query='corona'
INFO:root:rank=0 pagerank=4.5861e-03 url=www.lawfareblog.com/lawfare-podcast-united-nations-and-coronavirus-crisis
INFO:root:rank=1 pagerank=4.0460e-03 url=www.lawfareblog.com/house-oversight-committee-holds-day-two-hearing-government-coronavirus-response
INFO:root:rank=2 pagerank=2.6116e-03 url=www.lawfareblog.com/britains-coronavirus-response
INFO:root:rank=3 pagerank=2.5390e-03 url=www.lawfareblog.com/prosecuting-purposeful-coronavirus-exposure-terrorism
INFO:root:rank=4 pagerank=2.3557e-03 url=www.lawfareblog.com/israeli-emergency-regulations-location-tracking-coronavirus-carriers
INFO:root:rank=5 pagerank=2.2895e-03 url=www.lawfareblog.com/why-congress-conducting-business-usual-face-coronavirus
INFO:root:rank=6 pagerank=2.2727e-03 url=www.lawfareblog.com/livestream-house-oversight-committee-holds-hearing-government-coronavirus-response
INFO:root:rank=7 pagerank=2.2520e-03 url=www.lawfareblog.com/congressional-homeland-security-committees-seek-ways-support-state-federal-responses-coronavirus
INFO:root:rank=8 pagerank=2.1878e-03 url=www.lawfareblog.com/paper-hearing-experts-debate-digital-contact-tracing-and-coronavirus-privacy-concerns
INFO:root:rank=9 pagerank=2.0339e-03 url=www.lawfareblog.com/cyberlaw-podcast-how-israel-fighting-coronavirus

$ python3 pagerank.py --data=./lawfareblog.csv.gz --search_query='trump'
INFO:root:rank=0 pagerank=6.6243e-02 url=www.lawfareblog.com/donald-trump-and-politically-weaponized-executive-branch
INFO:root:rank=1 pagerank=6.0194e-02 url=www.lawfareblog.com/trump-asks-supreme-court-stay-congressional-subpeona-tax-returns
INFO:root:rank=2 pagerank=3.4969e-02 url=www.lawfareblog.com/trump-administrations-worrying-new-policy-israeli-settlements
INFO:root:rank=3 pagerank=3.2193e-02 url=www.lawfareblog.com/document-trump-revokes-obama-executive-order-counterterrorism-strike-casualty-reporting
INFO:root:rank=4 pagerank=3.0971e-02 url=www.lawfareblog.com/dc-circuit-overrules-district-courts-due-process-ruling-qasim-v-trump
INFO:root:rank=5 pagerank=2.8460e-02 url=www.lawfareblog.com/how-trumps-approach-middle-east-ignores-past-future-and-human-condition
INFO:root:rank=6 pagerank=2.5252e-02 url=www.lawfareblog.com/why-trump-cant-buy-greenland
INFO:root:rank=7 pagerank=2.2457e-02 url=www.lawfareblog.com/oral-argument-summary-qassim-v-trump
INFO:root:rank=8 pagerank=2.1462e-02 url=www.lawfareblog.com/dc-circuit-court-denies-trump-rehearing-mazars-case
INFO:root:rank=9 pagerank=2.1103e-02 url=www.lawfareblog.com/second-circuit-rules-mazars-must-hand-over-trump-tax-returns-new-york-prosecutors

$ python3 pagerank.py --data=./lawfareblog.csv.gz --search_query='iran'
INFO:root:rank=0 pagerank=6.6131e-02 url=www.lawfareblog.com/praise-presidents-iran-tweets
INFO:root:rank=1 pagerank=2.9199e-02 url=www.lawfareblog.com/how-us-iran-tensions-could-disrupt-iraqs-fragile-peace
INFO:root:rank=2 pagerank=1.7709e-02 url=www.lawfareblog.com/cyber-command-operational-update-clarifying-june-2019-iran-operation
INFO:root:rank=3 pagerank=1.4604e-02 url=www.lawfareblog.com/aborted-iran-strike-fine-line-between-necessity-and-revenge
INFO:root:rank=4 pagerank=8.4512e-03 url=www.lawfareblog.com/iranian-hostage-crisis-and-its-effect-american-politics
INFO:root:rank=5 pagerank=8.3989e-03 url=www.lawfareblog.com/parsing-state-departments-letter-use-force-against-iran
INFO:root:rank=6 pagerank=8.2581e-03 url=www.lawfareblog.com/announcing-united-states-and-use-force-against-iran-new-lawfare-e-book
INFO:root:rank=7 pagerank=8.0561e-03 url=www.lawfareblog.com/trump-moves-cut-irans-oil-revenues-whats-his-endgame
INFO:root:rank=8 pagerank=7.1939e-03 url=www.lawfareblog.com/us-names-iranian-revolutionary-guard-terrorist-organization-and-sanctions-international-criminal
INFO:root:rank=9 pagerank=5.9405e-03 url=www.lawfareblog.com/iran-shoots-down-us-drone-domestic-and-international-legal-implications
```

**Part 3:**
The webgraph of lawfareblog.com (the P matrix) naturally contains a lot of structure.
For example, essentially all pages on the domain have links to the root page https://lawfareblog.com/  and similarly broad pages like https://www.lawfareblog.com/topics and https://www.lawfareblog.com/subscribe-lawfare.
These pages therefore have a large pagerank.

Below are some high-pagerank pages on the Lawfareblog site:
```
$ python3 pagerank.py --data=./lawfareblog.csv.gz
INFO:root:rank=0 pagerank=8.4156e+00 url=www.lawfareblog.com/our-comments-policy
INFO:root:rank=1 pagerank=8.4156e+00 url=www.lawfareblog.com/lawfare-job-board
INFO:root:rank=2 pagerank=8.4156e+00 url=www.lawfareblog.com/litigation-documents-resources-related-travel-ban
INFO:root:rank=3 pagerank=8.4156e+00 url=www.lawfareblog.com/subscribe-lawfare
INFO:root:rank=4 pagerank=8.4156e+00 url=www.lawfareblog.com/support-lawfare
INFO:root:rank=5 pagerank=8.4156e+00 url=www.lawfareblog.com/upcoming-events
INFO:root:rank=6 pagerank=8.4156e+00 url=www.lawfareblog.com/snowden-revelations
INFO:root:rank=7 pagerank=8.4156e+00 url=www.lawfareblog.com/about-lawfare-brief-history-term-and-site
INFO:root:rank=8 pagerank=8.4156e+00 url=www.lawfareblog.com/topics
INFO:root:rank=9 pagerank=8.4156e+00 url=www.lawfareblog.com/documents-related-mueller-investigation
```

These pages are not very interesting, however, because they are not articles.
To find the most important articles, I modified the P matrix by removing all links to non-article pages.

How do we know if a link is a non-article page?
One method is to remove all pages that have "too many" links.
The `--filter_ratio` argument removes all pages that have more links than the specified fraction.

Using this option, I could estimate the most important articles on the domain with the following command:
```
$ python3 pagerank.py --data=./lawfareblog.csv.gz --filter_ratio=0.2
INFO:root:rank=0 pagerank=4.2773e+00 url=www.lawfareblog.com/trump-asks-supreme-court-stay-congressional-subpeona-tax-returns
INFO:root:rank=1 pagerank=2.7717e+00 url=www.lawfareblog.com/livestream-nov-21-impeachment-hearings-0
INFO:root:rank=2 pagerank=2.7533e+00 url=www.lawfareblog.com/opening-statement-david-holmes
INFO:root:rank=3 pagerank=1.8720e+00 url=www.lawfareblog.com/senate-examines-threats-homeland
INFO:root:rank=4 pagerank=1.7417e+00 url=www.lawfareblog.com/what-make-first-day-impeachment-hearings
INFO:root:rank=5 pagerank=1.7411e+00 url=www.lawfareblog.com/livestream-house-armed-services-committee-hearing-f-35-program
INFO:root:rank=6 pagerank=1.7347e+00 url=www.lawfareblog.com/whats-house-resolution-impeachment
INFO:root:rank=7 pagerank=1.6384e+00 url=www.lawfareblog.com/congress-us-policy-toward-syria-and-turkey-overview-recent-hearings
INFO:root:rank=8 pagerank=1.5597e+00 url=www.lawfareblog.com/summary-david-holmess-deposition-testimony
INFO:root:rank=9 pagerank=9.1265e-01 url=www.lawfareblog.com/events
```

As seen in the output, this last command removed the blog's most popular article (www.lawfareblog.com/snowden-revelations).
The blog editors believe that Snowden's revelations about NSA spying are so important that they link to the article on every single webpage in the domain,
and out "anti-spam" `--filter-ratio` argument removed this article from the list.

**Part 4:**
The runtime of pagerank depends heavily on the eigengap of the $\bar\bar P$ matrix,
and that this eigengap is bounded by the alpha parameter.

The next few commands below demonstrate the result of how alpha and filter ratios change both conversion times and outputs:
```
$ python3 pagerank.py --data=./lawfareblog.csv.gz --verbose 
DEBUG:root:computing indices
DEBUG:root:computing values
INFO:root:rank=0 pagerank=8.4156e+00 url=www.lawfareblog.com/our-comments-policy
INFO:root:rank=1 pagerank=8.4156e+00 url=www.lawfareblog.com/lawfare-job-board
INFO:root:rank=2 pagerank=8.4156e+00 url=www.lawfareblog.com/litigation-documents-resources-related-travel-ban
INFO:root:rank=3 pagerank=8.4156e+00 url=www.lawfareblog.com/subscribe-lawfare
INFO:root:rank=4 pagerank=8.4156e+00 url=www.lawfareblog.com/support-lawfare
INFO:root:rank=5 pagerank=8.4156e+00 url=www.lawfareblog.com/upcoming-events
INFO:root:rank=6 pagerank=8.4156e+00 url=www.lawfareblog.com/snowden-revelations
INFO:root:rank=7 pagerank=8.4156e+00 url=www.lawfareblog.com/about-lawfare-brief-history-term-and-site
INFO:root:rank=8 pagerank=8.4156e+00 url=www.lawfareblog.com/topics
INFO:root:rank=9 pagerank=8.4156e+00 url=www.lawfareblog.com/documents-related-mueller-investigation

$ python3 pagerank.py --data=./lawfareblog.csv.gz --verbose --alpha=0.99999
DEBUG:root:computing indices
DEBUG:root:computing values
INFO:root:rank=0 pagerank=1.0191e+01 url=www.lawfareblog.com/about-lawfare-brief-history-term-and-site
INFO:root:rank=1 pagerank=1.0191e+01 url=www.lawfareblog.com/lawfare-job-board
INFO:root:rank=2 pagerank=1.0191e+01 url=www.lawfareblog.com/litigation-documents-related-appointment-matthew-whitaker-acting-attorney-general
INFO:root:rank=3 pagerank=1.0191e+01 url=www.lawfareblog.com/litigation-documents-resources-related-travel-ban
INFO:root:rank=4 pagerank=1.0191e+01 url=www.lawfareblog.com/subscribe-lawfare
INFO:root:rank=5 pagerank=1.0191e+01 url=www.lawfareblog.com/topics
INFO:root:rank=6 pagerank=1.0191e+01 url=www.lawfareblog.com/masthead
INFO:root:rank=7 pagerank=1.0191e+01 url=www.lawfareblog.com/our-comments-policy
INFO:root:rank=8 pagerank=1.0191e+01 url=www.lawfareblog.com/upcoming-events
INFO:root:rank=9 pagerank=1.0191e+01 url=www.lawfareblog.com/documents-related-mueller-investigation

$ python3 pagerank.py --data=./lawfareblog.csv.gz --verbose --filter_ratio=0.2
DEBUG:root:computing indices
DEBUG:root:computing values
INFO:root:rank=0 pagerank=4.2773e+00 url=www.lawfareblog.com/trump-asks-supreme-court-stay-congressional-subpeona-tax-returns
INFO:root:rank=1 pagerank=2.7717e+00 url=www.lawfareblog.com/livestream-nov-21-impeachment-hearings-0
INFO:root:rank=2 pagerank=2.7533e+00 url=www.lawfareblog.com/opening-statement-david-holmes
INFO:root:rank=3 pagerank=1.8720e+00 url=www.lawfareblog.com/senate-examines-threats-homeland
INFO:root:rank=4 pagerank=1.7417e+00 url=www.lawfareblog.com/what-make-first-day-impeachment-hearings
INFO:root:rank=5 pagerank=1.7411e+00 url=www.lawfareblog.com/livestream-house-armed-services-committee-hearing-f-35-program
INFO:root:rank=6 pagerank=1.7347e+00 url=www.lawfareblog.com/whats-house-resolution-impeachment
INFO:root:rank=7 pagerank=1.6384e+00 url=www.lawfareblog.com/congress-us-policy-toward-syria-and-turkey-overview-recent-hearings
INFO:root:rank=8 pagerank=1.5597e+00 url=www.lawfareblog.com/summary-david-holmess-deposition-testimony
INFO:root:rank=9 pagerank=9.1265e-01 url=www.lawfareblog.com/events

$ python3 pagerank.py --data=./lawfareblog.csv.gz --verbose --filter_ratio=0.2 --alpha=0.99999
DEBUG:root:computing indices
DEBUG:root:computing values
INFO:root:rank=0 pagerank=4.7947e+01 url=www.lawfareblog.com/covid-19-speech-and-surveillance-response
INFO:root:rank=1 pagerank=4.7947e+01 url=www.lawfareblog.com/lawfare-live-covid-19-speech-and-surveillance
INFO:root:rank=2 pagerank=7.2709e+00 url=www.lawfareblog.com/cost-using-zero-days
INFO:root:rank=3 pagerank=2.1691e+00 url=www.lawfareblog.com/lawfare-podcast-former-congressman-brian-baird-and-daniel-schuman-how-congress-can-continue-function
INFO:root:rank=4 pagerank=1.4214e+00 url=www.lawfareblog.com/events
INFO:root:rank=5 pagerank=1.0863e+00 url=www.lawfareblog.com/water-wars-increased-us-focus-indo-pacific
INFO:root:rank=6 pagerank=1.0863e+00 url=www.lawfareblog.com/water-wars-drill-maybe-drill
INFO:root:rank=7 pagerank=1.0863e+00 url=www.lawfareblog.com/water-wars-disjointed-operations-south-china-sea
INFO:root:rank=8 pagerank=1.0863e+00 url=www.lawfareblog.com/water-wars-us-china-divide-shangri-la
INFO:root:rank=9 pagerank=1.0863e+00 url=www.lawfareblog.com/water-wars-song-oil-and-fire
```

Which of these is "better" is, of course, subjective.
There is a tradeoff between quality answers and algorithmic runtime.

## The Personalization Vector

**Part 1:**
Using the `--personalization_vector_query` option will result in webpages being deemed "important" if other related websites also find them so. 
Using the `--search_query` option, on the other hand, will classify "important" websites as those deemed so by ANY other website.

Each, therefore, will give different results.
Below is a demonstration.

Using the Personalization Vector option with keyword `corona`:
```
$ python3 pagerank.py --data=./lawfareblog.csv.gz --filter_ratio=0.2 --personalization_vector_query='corona'
INFO:root:rank=0 pagerank=8.8870e-01 url=www.lawfareblog.com/covid-19-speech-and-surveillance-response
INFO:root:rank=1 pagerank=8.8867e-01 url=www.lawfareblog.com/lawfare-live-covid-19-speech-and-surveillance
INFO:root:rank=2 pagerank=1.8256e-01 url=www.lawfareblog.com/chinatalk-how-party-takes-its-propaganda-global
INFO:root:rank=3 pagerank=1.4907e-01 url=www.lawfareblog.com/brexit-not-immune-coronavirus
INFO:root:rank=4 pagerank=1.4907e-01 url=www.lawfareblog.com/rational-security-my-corona-edition
INFO:root:rank=5 pagerank=1.0729e-01 url=www.lawfareblog.com/trump-cant-reopen-country-over-state-objections
INFO:root:rank=6 pagerank=1.0199e-01 url=www.lawfareblog.com/prosecuting-purposeful-coronavirus-exposure-terrorism
INFO:root:rank=7 pagerank=1.0199e-01 url=www.lawfareblog.com/britains-coronavirus-response
INFO:root:rank=8 pagerank=9.4298e-02 url=www.lawfareblog.com/lawfare-podcast-mom-and-dad-talk-clinical-trials-pandemic
INFO:root:rank=9 pagerank=8.7207e-02 url=www.lawfareblog.com/house-oversight-committee-holds-day-two-hearing-government-coronavirus-response
```

Using the Search Query Option with keyword `corona`:
```
$ python3 pagerank.py --data=./lawfareblog.csv.gz --filter_ratio=0.2 --search_query='corona'
INFO:root:rank=0 pagerank=1.1602e-01 url=www.lawfareblog.com/congress-needs-coronavirus-failsafe-its-too-late
INFO:root:rank=1 pagerank=5.6374e-02 url=www.lawfareblog.com/house-oversight-committee-holds-day-two-hearing-government-coronavirus-response
INFO:root:rank=2 pagerank=5.0830e-02 url=www.lawfareblog.com/britains-coronavirus-response
INFO:root:rank=3 pagerank=5.0481e-02 url=www.lawfareblog.com/prosecuting-purposeful-coronavirus-exposure-terrorism
INFO:root:rank=4 pagerank=4.8031e-02 url=www.lawfareblog.com/livestream-house-oversight-committee-holds-hearing-government-coronavirus-response
INFO:root:rank=5 pagerank=4.7743e-02 url=www.lawfareblog.com/paper-hearing-experts-debate-digital-contact-tracing-and-coronavirus-privacy-concerns
INFO:root:rank=6 pagerank=4.3727e-02 url=www.lawfareblog.com/why-congress-conducting-business-usual-face-coronavirus
INFO:root:rank=7 pagerank=2.5817e-02 url=www.lawfareblog.com/israeli-emergency-regulations-location-tracking-coronavirus-carriers
INFO:root:rank=8 pagerank=2.5463e-02 url=www.lawfareblog.com/lawfare-podcast-united-nations-and-coronavirus-crisis
INFO:root:rank=9 pagerank=1.9066e-02 url=www.lawfareblog.com/congressional-homeland-security-committees-seek-ways-support-state-federal-responses-coronavirus
```

**Part 2:**
Another use of the `--personalization_vector_query` option is that we can find out what webpages are related to the coronavirus without directly mentioning it.
This can be used to map out what types of topics are similar to the coronavirus.

For example, the following query ranks all webpages by their `corona` importance,
but removes webpages mentioning `corona` from the results:
```
$ python3 pagerank.py --data=./lawfareblog.csv.gz --filter_ratio=0.2 --personalization_vector_query='corona' --search_query='-corona'
INFO:root:rank=0 pagerank=8.8870e-01 url=www.lawfareblog.com/covid-19-speech-and-surveillance-response
INFO:root:rank=1 pagerank=8.8867e-01 url=www.lawfareblog.com/lawfare-live-covid-19-speech-and-surveillance
INFO:root:rank=2 pagerank=1.8256e-01 url=www.lawfareblog.com/chinatalk-how-party-takes-its-propaganda-global
INFO:root:rank=3 pagerank=1.0729e-01 url=www.lawfareblog.com/trump-cant-reopen-country-over-state-objections
INFO:root:rank=4 pagerank=9.4298e-02 url=www.lawfareblog.com/lawfare-podcast-mom-and-dad-talk-clinical-trials-pandemic
INFO:root:rank=5 pagerank=7.9633e-02 url=www.lawfareblog.com/fault-lines-foreign-policy-quarantined
INFO:root:rank=6 pagerank=7.5307e-02 url=www.lawfareblog.com/limits-world-health-organization
INFO:root:rank=7 pagerank=6.8115e-02 url=www.lawfareblog.com/chinatalk-dispatches-shanghai-beijing-and-hong-kong
INFO:root:rank=8 pagerank=6.4847e-02 url=www.lawfareblog.com/us-moves-dismiss-case-against-company-linked-ira-troll-farm
INFO:root:rank=9 pagerank=6.4847e-02 url=www.lawfareblog.com/livestream-house-armed-services-holds-hearing-national-security-challenges-north-and-south-america
```
You can see that there are many urls about concepts that are obviously related like "covid", "clinical trials", and "quarantine",
but this algorithm also finds articles about Chinese propaganda and Trump's policy decisions.
Both of these articles are highly relevant to coronavirus discussions,
but a simple keyword search for corona or related terms would not find these articles.

**Part 3:**

Below, I further used the Personalization Vector to search for articles relevant to a certain topic without the explicit use of the keyword. 

`globalization`
```
$ python3 pagerank.py --data=./lawfareblog.csv.gz --filter_ratio=0.2 --personalization_vector_query='globalization' --search_query='-globalization'
INFO:root:rank=0 pagerank=1.2483e-01 url=www.lawfareblog.com/bathtub-fallacy-and-risks-terrorism
INFO:root:rank=1 pagerank=1.2483e-01 url=www.lawfareblog.com/what-extent-self-defense-state-0
INFO:root:rank=2 pagerank=1.2483e-01 url=www.lawfareblog.com/readings-post-syria-extracted-onion
INFO:root:rank=3 pagerank=1.2483e-01 url=www.lawfareblog.com/how-declare-war-anno-domini-1429-2017-repost
INFO:root:rank=4 pagerank=1.2483e-01 url=www.lawfareblog.com/alan-z-rozenshtein-digital-communications-and-data-storage-companies-surveillance-intermediaries
INFO:root:rank=5 pagerank=1.7659e-02 url=www.lawfareblog.com/foreign-policy-essay-can-terrorists-be-scared-straight
INFO:root:rank=6 pagerank=1.1796e-02 url=www.lawfareblog.com/depoliticizing-foreign-interference
INFO:root:rank=7 pagerank=1.1794e-02 url=www.lawfareblog.com/trinidads-islamic-state-problem
INFO:root:rank=8 pagerank=1.1794e-02 url=www.lawfareblog.com/fractured-terrorism-threat-america
INFO:root:rank=9 pagerank=1.1794e-02 url=www.lawfareblog.com/chinas-advance-antarctic
```

`japan`
```
$ python3 pagerank.py --data=./lawfareblog.csv.gz --filter_ratio=0.2 --personalization_vector_query='japan' --search_query='-japan'
INFO:root:rank=0 pagerank=1.4824e-01 url=www.lawfareblog.com/water-wars-disjointed-operations-south-china-sea
INFO:root:rank=1 pagerank=1.3843e-01 url=www.lawfareblog.com/water-wars-increased-us-focus-indo-pacific
INFO:root:rank=2 pagerank=1.3841e-01 url=www.lawfareblog.com/water-wars-drill-maybe-drill
INFO:root:rank=3 pagerank=1.3828e-01 url=www.lawfareblog.com/water-wars-us-china-divide-shangri-la
INFO:root:rank=4 pagerank=1.3828e-01 url=www.lawfareblog.com/water-wars-sinking-feeling-philippine-china-relations
INFO:root:rank=5 pagerank=1.3828e-01 url=www.lawfareblog.com/water-wars-song-oil-and-fire
INFO:root:rank=6 pagerank=4.1869e-02 url=www.lawfareblog.com/fleeing-oceans
INFO:root:rank=7 pagerank=4.1869e-02 url=www.lawfareblog.com/frankenstein-200
INFO:root:rank=8 pagerank=4.1869e-02 url=www.lawfareblog.com/irony-white-house-warriors
INFO:root:rank=9 pagerank=4.1869e-02 url=www.lawfareblog.com/partisan-politics-and-federal-law-enforcement-promise-and-corruption-reconstruction
```
`apple`
```
$ python3 pagerank.py --data=./lawfareblog.csv.gz --filter_ratio=0.2 --personalization_vector_query='apple' --search_query='-apple'
INFO:root:rank=0 pagerank=3.8882e-01 url=www.lawfareblog.com/thoughtful-response-going-dark-and-child-pornography-issue
INFO:root:rank=1 pagerank=3.6751e-01 url=www.lawfareblog.com/whatsapp-nso-group-lawsuit-and-limits-lawful-hacking
INFO:root:rank=2 pagerank=3.2404e-01 url=www.lawfareblog.com/rethinking-encryption
INFO:root:rank=3 pagerank=3.2266e-01 url=www.lawfareblog.com/focusing-privacy-wont-solve-facebooks-problems
INFO:root:rank=4 pagerank=3.1944e-01 url=www.lawfareblog.com/encryption-and-combating-child-exploitation-imagery
INFO:root:rank=5 pagerank=2.1903e-01 url=www.lawfareblog.com/child-exploitation-and-future-encryption
INFO:root:rank=6 pagerank=2.0825e-01 url=www.lawfareblog.com/cost-using-zero-days
INFO:root:rank=7 pagerank=1.4040e-01 url=www.lawfareblog.com/trump-asks-supreme-court-stay-congressional-subpeona-tax-returns
INFO:root:rank=8 pagerank=1.0550e-01 url=www.lawfareblog.com/doj-charges-two-former-twitter-employees-spying-saudis
INFO:root:rank=9 pagerank=9.4536e-02 url=www.lawfareblog.com/opening-statement-david-holmes
```




## Web Search with word2vec

### Task 1

```
$ python3 pagerank_word2vec.py --data=./lawfareblog.csv.gz --search_query='weapons'  
INFO:gensim.models.keyedvectors:precomputing L2-norms of word weight vectors
INFO:root:rank=0 pagerank=0.004571518860757351 url=www.lawfareblog.com/why-did-you-wait-moral-emptiness-and-drone-strikes
INFO:root:rank=1 pagerank=0.0031107424292713404 url=www.lawfareblog.com/dc-district-court-dismisses-journalists-drone-lawsuit
INFO:root:rank=2 pagerank=0.0020231129601597786 url=www.lawfareblog.com/revived-cia-drone-strike-program-comments-new-policy
INFO:root:rank=3 pagerank=0.0019667143933475018 url=www.lawfareblog.com/us-court-appeals-dc-circuit-dismisses-suit-over-us-drone-strike
INFO:root:rank=4 pagerank=0.001178761012852192 url=www.lawfareblog.com/iran-shoots-down-us-drone-domestic-and-international-legal-implications     
INFO:root:rank=5 pagerank=0.0011619674041867256 url=www.lawfareblog.com/slaughterbots-and-other-anticipated-autonomous-weapons-problems
INFO:root:rank=6 pagerank=0.0011276121949777007 url=www.lawfareblog.com/german-courts-weigh-legal-responsibility-us-drone-strikes
INFO:root:rank=7 pagerank=0.0008373793680220842 url=www.lawfareblog.com/shift-jsoc-drone-strikes-does-not-mean-cia-has-been-sidelined
INFO:root:rank=8 pagerank=0.0007870369008742273 url=www.lawfareblog.com/atomwaffen-division-member-pleads-guilty-firearms-charge
INFO:root:rank=9 pagerank=0.0007856971933506429 url=www.lawfareblog.com/waiving-imminent-threat-test-cia-drone-strikes-pakistan
```


**Part 2:**
```
$ python3 pagerank_word2vec.py --data=./lawfareblog.csv.gz --search_query='corona'
INFO:gensim.models.keyedvectors:precomputing L2-norms of word weight vectors
INFO:root:rank=0 pagerank=0.001003776676952839 url=www.lawfareblog.com/lawfare-podcast-united-nations-and-coronavirus-crisis
INFO:root:rank=1 pagerank=0.0008922395063564181 url=www.lawfareblog.com/house-oversight-committee-holds-day-two-hearing-government-coronavirus-response
INFO:root:rank=2 pagerank=0.0007039029151201248 url=www.lawfareblog.com/britains-coronavirus-response
INFO:root:rank=3 pagerank=0.0006915341946296394 url=www.lawfareblog.com/prosecuting-purposeful-coronavirus-exposure-terrorism
INFO:root:rank=4 pagerank=0.000670412031468004 url=www.lawfareblog.com/israeli-emergency-regulations-location-tracking-coronavirus-carriers
INFO:root:rank=5 pagerank=0.0006625585374422371 url=www.lawfareblog.com/why-congress-conducting-business-usual-face-coronavirus
INFO:root:rank=6 pagerank=0.0006504578050225973 url=www.lawfareblog.com/congressional-homeland-security-committees-seek-ways-support-state-federal-responses-coronavirus
INFO:root:rank=7 pagerank=0.0006361958803609014 url=www.lawfareblog.com/paper-hearing-experts-debate-digital-contact-tracing-and-coronavirus-privacy-concerns
INFO:root:rank=8 pagerank=0.000612482544966042 url=www.lawfareblog.com/house-subcommittee-voices-concerns-over-us-management-coronavirus
INFO:root:rank=9 pagerank=0.0006018723943270743 url=www.lawfareblog.com/livestream-house-oversight-committee-holds-hearing-government-coronavirus-response
```
```
$ python3 pagerank.py --data=./lawfareblog.csv.gz --search_query='trump'
INFO:gensim.models.keyedvectors:precomputing L2-norms of word weight vectors
INFO:root:rank=0 pagerank=0.005782557651400566 url=www.lawfareblog.com/trump-asks-supreme-court-stay-congressional-subpeona-tax-returns
INFO:root:rank=1 pagerank=0.005233839154243469 url=www.lawfareblog.com/document-trump-revokes-obama-executive-order-counterterrorism-strike-casualty-reporting
INFO:root:rank=2 pagerank=0.005129670724272728 url=www.lawfareblog.com/trump-administrations-worrying-new-policy-israeli-settlements
INFO:root:rank=3 pagerank=0.004659898113459349 url=www.lawfareblog.com/dc-circuit-overrules-district-courts-due-process-ruling-qasim-v-trump
INFO:root:rank=4 pagerank=0.004593398422002792 url=www.lawfareblog.com/donald-trump-and-politically-weaponized-executive-branch
INFO:root:rank=5 pagerank=0.004307133611291647 url=www.lawfareblog.com/how-trumps-approach-middle-east-ignores-past-future-and-human-condition
INFO:root:rank=6 pagerank=0.0040934765711426735 url=www.lawfareblog.com/why-trump-cant-buy-greenland
INFO:root:rank=7 pagerank=0.0037590833380818367 url=www.lawfareblog.com/oral-argument-summary-qassim-v-trump
INFO:root:rank=8 pagerank=0.003450872143730521 url=www.lawfareblog.com/dc-circuit-court-denies-trump-rehearing-mazars-case
INFO:root:rank=9 pagerank=0.0034484383650124073 url=www.lawfareblog.com/second-circuit-rules-mazars-must-hand-over-trump-tax-returns-new-york-prosecutors
```
```
$ python3 pagerank.py --data=./lawfareblog.csv.gz --search_query='iran'
INFO:gensim.models.keyedvectors:precomputing L2-norms of word weight vectors
INFO:root:rank=0 pagerank=0.005129670724272728 url=www.lawfareblog.com/trump-administrations-worrying-new-policy-israeli-settlements
INFO:root:rank=1 pagerank=0.005016769748181105 url=www.lawfareblog.com/update-military-commissions-continued-health-issues-recusal-motion-and-new-cell-al-iraqi
INFO:root:rank=2 pagerank=0.004574609454721212 url=www.lawfareblog.com/praise-presidents-iran-tweets
INFO:root:rank=3 pagerank=0.004417411983013153 url=www.lawfareblog.com/how-us-iran-tensions-could-disrupt-iraqs-fragile-peace
INFO:root:rank=4 pagerank=0.004365929868072271 url=www.lawfareblog.com/haftar-attacking-tripoli-us-needs-re-engage-libya
INFO:root:rank=5 pagerank=0.0034236714709550142 url=www.lawfareblog.com/france-makes-play-try-foreign-fighters-iraq
INFO:root:rank=6 pagerank=0.0026927897706627846 url=www.lawfareblog.com/cyber-command-operational-update-clarifying-june-2019-iran-operation       
INFO:root:rank=7 pagerank=0.002256679581478238 url=www.lawfareblog.com/document-sens-kaine-and-young-introduce-bill-revoke-iraq-force-authorizations
INFO:root:rank=8 pagerank=0.0019391420064494014 url=www.lawfareblog.com/aborted-iran-strike-fine-line-between-necessity-and-revenge
INFO:root:rank=9 pagerank=0.0018262730445712805 url=www.lawfareblog.com/its-not-only-iraq-and-syria
```

**Part 3:**

```
$ python3 pagerank.py --data=./lawfareblog.csv.gz
INFO:root:rank=0 pagerank=0.2874051630496979 url=www.lawfareblog.com/about-lawfare-brief-history-term-and-site
INFO:root:rank=1 pagerank=0.2874051630496979 url=www.lawfareblog.com/lawfare-job-board
INFO:root:rank=2 pagerank=0.2874051630496979 url=www.lawfareblog.com/litigation-documents-resources-related-travel-ban
INFO:root:rank=3 pagerank=0.2874051630496979 url=www.lawfareblog.com/subscribe-lawfare
INFO:root:rank=4 pagerank=0.2874051630496979 url=www.lawfareblog.com/our-comments-policy
INFO:root:rank=5 pagerank=0.2874051630496979 url=www.lawfareblog.com/upcoming-events
INFO:root:rank=6 pagerank=0.2874051630496979 url=www.lawfareblog.com/support-lawfare
INFO:root:rank=7 pagerank=0.2874051630496979 url=www.lawfareblog.com/snowden-revelations
INFO:root:rank=8 pagerank=0.2874051630496979 url=www.lawfareblog.com/topics
INFO:root:rank=9 pagerank=0.2874051630496979 url=www.lawfareblog.com/documents-related-mueller-investigation
```

```
$ python3 pagerank.py --data=./lawfareblog.csv.gz --filter_ratio=0.2
INFO:root:rank=0 pagerank=0.3469613492488861 url=www.lawfareblog.com/trump-asks-supreme-court-stay-congressional-subpeona-tax-returns
INFO:root:rank=1 pagerank=0.29521211981773376 url=www.lawfareblog.com/livestream-nov-21-impeachment-hearings-0
INFO:root:rank=2 pagerank=0.29039666056632996 url=www.lawfareblog.com/opening-statement-david-holmes
INFO:root:rank=3 pagerank=0.15178653597831726 url=www.lawfareblog.com/lawfare-podcast-ben-nimmo-whack-mole-game-disinformation
INFO:root:rank=4 pagerank=0.15098513662815094 url=www.lawfareblog.com/todays-headlines-and-commentary-1964
INFO:root:rank=5 pagerank=0.15098513662815094 url=www.lawfareblog.com/todays-headlines-and-commentary-1963
INFO:root:rank=6 pagerank=0.15071173012256622 url=www.lawfareblog.com/lawfare-podcast-week-was-impeachment
INFO:root:rank=7 pagerank=0.14956679940223694 url=www.lawfareblog.com/todays-headlines-and-commentary-1962
INFO:root:rank=8 pagerank=0.14366623759269714 url=www.lawfareblog.com/cyberlaw-podcast-mistrusting-google
INFO:root:rank=9 pagerank=0.14239734411239624 url=www.lawfareblog.com/lawfare-podcast-bonus-edition-gordon-sondland-vs-committee-no-bull

```

```
$ python3 pagerank.py --data=./lawfareblog.csv.gz --verbose 
DEBUG:root:computing indices
DEBUG:root:computing values
DEBUG:root:i=0 accuracy=tensor(1.3794)
DEBUG:root:i=1 accuracy=tensor(0.1164)
DEBUG:root:i=2 accuracy=tensor(0.0750)
DEBUG:root:i=3 accuracy=tensor(0.0317)
DEBUG:root:i=4 accuracy=tensor(0.0175)
DEBUG:root:i=5 accuracy=tensor(0.0085)
DEBUG:root:i=6 accuracy=tensor(0.0044)
DEBUG:root:i=7 accuracy=tensor(0.0022)
DEBUG:root:i=8 accuracy=tensor(0.0011)
DEBUG:root:i=9 accuracy=tensor(0.0006)
DEBUG:root:i=10 accuracy=tensor(0.0003)
DEBUG:root:i=11 accuracy=tensor(0.0001)
DEBUG:root:i=12 accuracy=tensor(7.1499e-05)
DEBUG:root:i=13 accuracy=tensor(3.4340e-05)
DEBUG:root:i=14 accuracy=tensor(1.5639e-05)
DEBUG:root:i=15 accuracy=tensor(6.3711e-06)
DEBUG:root:i=16 accuracy=tensor(2.8937e-06)
DEBUG:root:i=17 accuracy=tensor(1.3563e-06)
DEBUG:root:i=18 accuracy=tensor(4.3141e-07)
INFO:root:rank=0 pagerank=0.2874051630496979 url=www.lawfareblog.com/about-lawfare-brief-history-term-and-site
INFO:root:rank=1 pagerank=0.2874051630496979 url=www.lawfareblog.com/lawfare-job-board
INFO:root:rank=2 pagerank=0.2874051630496979 url=www.lawfareblog.com/litigation-documents-resources-related-travel-ban
INFO:root:rank=3 pagerank=0.2874051630496979 url=www.lawfareblog.com/subscribe-lawfare
INFO:root:rank=4 pagerank=0.2874051630496979 url=www.lawfareblog.com/our-comments-policy
INFO:root:rank=5 pagerank=0.2874051630496979 url=www.lawfareblog.com/upcoming-events
INFO:root:rank=6 pagerank=0.2874051630496979 url=www.lawfareblog.com/support-lawfare
INFO:root:rank=7 pagerank=0.2874051630496979 url=www.lawfareblog.com/snowden-revelations
INFO:root:rank=8 pagerank=0.2874051630496979 url=www.lawfareblog.com/topics


$ python3 pagerank.py --data=./lawfareblog.csv.gz --verbose --alpha=0.99999
DEBUG:root:computing indices
DEBUG:root:computing values
DEBUG:root:i=0 accuracy=tensor(1.3846)
DEBUG:root:i=1 accuracy=tensor(0.0709)
DEBUG:root:i=2 accuracy=tensor(0.0188)
DEBUG:root:i=3 accuracy=tensor(0.0070)
DEBUG:root:i=4 accuracy=tensor(0.0027)
DEBUG:root:i=5 accuracy=tensor(0.0010)
DEBUG:root:i=6 accuracy=tensor(0.0004)
DEBUG:root:i=7 accuracy=tensor(0.0001)
DEBUG:root:i=8 accuracy=tensor(4.8224e-05)
DEBUG:root:i=9 accuracy=tensor(1.7173e-05)
DEBUG:root:i=10 accuracy=tensor(6.1184e-06)
DEBUG:root:i=11 accuracy=tensor(2.1724e-06)
DEBUG:root:i=12 accuracy=tensor(7.7635e-07)
INFO:root:rank=0 pagerank=0.28859302401542664 url=www.lawfareblog.com/documents-related-mueller-investigation
INFO:root:rank=1 pagerank=0.28859302401542664 url=www.lawfareblog.com/lawfare-job-board
INFO:root:rank=2 pagerank=0.28859302401542664 url=www.lawfareblog.com/litigation-documents-resources-related-travel-ban
INFO:root:rank=3 pagerank=0.28859302401542664 url=www.lawfareblog.com/subscribe-lawfare
INFO:root:rank=4 pagerank=0.28859302401542664 url=www.lawfareblog.com/about-lawfare-brief-history-term-and-site
INFO:root:rank=5 pagerank=0.28859302401542664 url=www.lawfareblog.com/our-comments-policy
INFO:root:rank=6 pagerank=0.28859302401542664 url=www.lawfareblog.com/upcoming-events
INFO:root:rank=7 pagerank=0.28859302401542664 url=www.lawfareblog.com/support-lawfare
INFO:root:rank=8 pagerank=0.28859302401542664 url=www.lawfareblog.com/snowden-revelations
INFO:root:rank=9 pagerank=0.28859302401542664 url=www.lawfareblog.com/topics


$ python3 pagerank.py --data=./lawfareblog.csv.gz --verbose --filter_ratio=0.2
DEBUG:root:computing indices
DEBUG:root:computing values
DEBUG:root:i=0 accuracy=tensor(1.2610)
DEBUG:root:i=1 accuracy=tensor(0.4986)
DEBUG:root:i=2 accuracy=tensor(0.1342)
DEBUG:root:i=3 accuracy=tensor(0.0692)
DEBUG:root:i=4 accuracy=tensor(0.0234)
DEBUG:root:i=5 accuracy=tensor(0.0102)
DEBUG:root:i=6 accuracy=tensor(0.0049)
DEBUG:root:i=7 accuracy=tensor(0.0023)
DEBUG:root:i=8 accuracy=tensor(0.0011)
DEBUG:root:i=9 accuracy=tensor(0.0005)
DEBUG:root:i=10 accuracy=tensor(0.0003)
DEBUG:root:i=11 accuracy=tensor(0.0001)
DEBUG:root:i=12 accuracy=tensor(8.2410e-05)
DEBUG:root:i=13 accuracy=tensor(4.8200e-05)
DEBUG:root:i=14 accuracy=tensor(2.8818e-05)
DEBUG:root:i=15 accuracy=tensor(1.7413e-05)
DEBUG:root:i=16 accuracy=tensor(1.0548e-05)
DEBUG:root:i=17 accuracy=tensor(6.3828e-06)
DEBUG:root:i=18 accuracy=tensor(3.8365e-06)
DEBUG:root:i=19 accuracy=tensor(2.2997e-06)
DEBUG:root:i=20 accuracy=tensor(1.3622e-06)
DEBUG:root:i=21 accuracy=tensor(8.0970e-07)
INFO:root:rank=0 pagerank=0.3469613492488861 url=www.lawfareblog.com/trump-asks-supreme-court-stay-congressional-subpeona-tax-returns
INFO:root:rank=1 pagerank=0.29521211981773376 url=www.lawfareblog.com/livestream-nov-21-impeachment-hearings-0
INFO:root:rank=2 pagerank=0.29039666056632996 url=www.lawfareblog.com/opening-statement-david-holmes
INFO:root:rank=3 pagerank=0.15178653597831726 url=www.lawfareblog.com/lawfare-podcast-ben-nimmo-whack-mole-game-disinformation
INFO:root:rank=4 pagerank=0.15098513662815094 url=www.lawfareblog.com/todays-headlines-and-commentary-1964
INFO:root:rank=5 pagerank=0.15098513662815094 url=www.lawfareblog.com/todays-headlines-and-commentary-1963
INFO:root:rank=6 pagerank=0.15071173012256622 url=www.lawfareblog.com/lawfare-podcast-week-was-impeachment
INFO:root:rank=7 pagerank=0.14956679940223694 url=www.lawfareblog.com/todays-headlines-and-commentary-1962
INFO:root:rank=8 pagerank=0.14366623759269714 url=www.lawfareblog.com/cyberlaw-podcast-mistrusting-google
INFO:root:rank=9 pagerank=0.14239734411239624 url=www.lawfareblog.com/lawfare-podcast-bonus-edition-gordon-sondland-vs-committee-no-bull


$ python3 pagerank.py --data=./lawfareblog.csv.gz --verbose --filter_ratio=0.2 --alpha=0.99999
## Multiple DEBUG statements (686)
INFO:root:rank=0 pagerank=0.7014895677566528 url=www.lawfareblog.com/covid-19-speech-and-surveillance-response
INFO:root:rank=1 pagerank=0.7014873623847961 url=www.lawfareblog.com/lawfare-live-covid-19-speech-and-surveillance
INFO:root:rank=2 pagerank=0.10551629960536957 url=www.lawfareblog.com/cost-using-zero-days
INFO:root:rank=3 pagerank=0.03175504133105278 url=www.lawfareblog.com/lawfare-podcast-former-congressman-brian-baird-and-daniel-schuman-how-congress-can-continue-function
INFO:root:rank=4 pagerank=0.022039713338017464 url=www.lawfareblog.com/events
INFO:root:rank=5 pagerank=0.01602667197585106 url=www.lawfareblog.com/water-wars-increased-us-focus-indo-pacific
INFO:root:rank=6 pagerank=0.016025930643081665 url=www.lawfareblog.com/water-wars-drill-maybe-drill
INFO:root:rank=7 pagerank=0.016022762283682823 url=www.lawfareblog.com/water-wars-disjointed-operations-south-china-sea
INFO:root:rank=8 pagerank=0.016019931063055992 url=www.lawfareblog.com/water-wars-song-oil-and-fire
INFO:root:rank=9 pagerank=0.016019923612475395 url=www.lawfareblog.com/water-wars-sinking-feeling-philippine-china-relations
```

## The Personalization Vector

**Part 1:**
```
$ python3 pagerank.py --data=./lawfareblog.csv.gz --filter_ratio=0.2 --personalization_vector_query='corona'
INFO:gensim.models.keyedvectors:precomputing L2-norms of word weight vectors
INFO:root:rank=0 pagerank=0.6321316361427307 url=www.lawfareblog.com/covid-19-speech-and-surveillance-response
INFO:root:rank=1 pagerank=0.6321078538894653 url=www.lawfareblog.com/lawfare-live-covid-19-speech-and-surveillance
INFO:root:rank=2 pagerank=0.1496182531118393 url=www.lawfareblog.com/chinatalk-how-party-takes-its-propaganda-global
INFO:root:rank=3 pagerank=0.11625612527132034 url=www.lawfareblog.com/brexit-not-immune-coronavirus
INFO:root:rank=4 pagerank=0.11625612527132034 url=www.lawfareblog.com/rational-security-my-corona-edition
INFO:root:rank=5 pagerank=0.08883301168680191 url=www.lawfareblog.com/trump-cant-reopen-country-over-state-objections
INFO:root:rank=6 pagerank=0.08544307202100754 url=www.lawfareblog.com/prosecuting-purposeful-coronavirus-exposure-terrorism
INFO:root:rank=7 pagerank=0.08544307202100754 url=www.lawfareblog.com/britains-coronavirus-response
INFO:root:rank=8 pagerank=0.07188324630260468 url=www.lawfareblog.com/lawfare-podcast-united-nations-and-coronavirus-crisis
INFO:root:rank=9 pagerank=0.06896847486495972 url=www.lawfareblog.com/house-oversight-committee-holds-day-two-hearing-government-coronavirus-response


```

```
$ python3 pagerank.py --data=./lawfareblog.csv.gz --filter_ratio=0.2 --search_query='corona'
INFO:gensim.models.keyedvectors:precomputing L2-norms of word weight vectors
INFO:root:rank=0 pagerank=0.008131962269544601 url=www.lawfareblog.com/house-oversight-committee-holds-day-two-hearing-government-coronavirus-response
INFO:root:rank=1 pagerank=0.007790825795382261 url=www.lawfareblog.com/lawfare-podcast-united-nations-and-coronavirus-crisis
INFO:root:rank=2 pagerank=0.005226220469921827 url=www.lawfareblog.com/livestream-house-oversight-committee-holds-hearing-government-coronavirus-response
INFO:root:rank=3 pagerank=0.0039583845064044 url=www.lawfareblog.com/britains-coronavirus-response
INFO:root:rank=4 pagerank=0.003811446251347661 url=www.lawfareblog.com/prosecuting-purposeful-coronavirus-exposure-terrorism
INFO:root:rank=5 pagerank=0.003397284774109721 url=www.lawfareblog.com/paper-hearing-experts-debate-digital-contact-tracing-and-coronavirus-privacy-concerns
INFO:root:rank=6 pagerank=0.0033633282873779535 url=www.lawfareblog.com/cyberlaw-podcast-how-israel-fighting-coronavirus
INFO:root:rank=7 pagerank=0.0033556800335645676 url=www.lawfareblog.com/israeli-emergency-regulations-location-tracking-coronavirus-carriers
INFO:root:rank=8 pagerank=0.0032159897964447737 url=www.lawfareblog.com/congress-needs-coronavirus-failsafe-its-too-late
INFO:root:rank=9 pagerank=0.0031036492437124252 url=www.lawfareblog.com/why-congress-conducting-business-usual-face-coronavirus
```

**Part 2:**
```
$ python3 pagerank.py --data=./lawfareblog.csv.gz --filter_ratio=0.2 --personalization_vector_query='corona' --search_query='corona'
INFO:gensim.models.keyedvectors:precomputing L2-norms of word weight vectors
INFO:root:rank=0 pagerank=0.11625612527132034 url=www.lawfareblog.com/brexit-not-immune-coronavirus
INFO:root:rank=1 pagerank=0.11625612527132034 url=www.lawfareblog.com/rational-security-my-corona-edition
INFO:root:rank=2 pagerank=0.08544307202100754 url=www.lawfareblog.com/prosecuting-purposeful-coronavirus-exposure-terrorism
INFO:root:rank=3 pagerank=0.08544307202100754 url=www.lawfareblog.com/britains-coronavirus-response
INFO:root:rank=4 pagerank=0.07188324630260468 url=www.lawfareblog.com/lawfare-podcast-united-nations-and-coronavirus-crisis
INFO:root:rank=5 pagerank=0.06896847486495972 url=www.lawfareblog.com/house-oversight-committee-holds-day-two-hearing-government-coronavirus-response
INFO:root:rank=6 pagerank=0.06561189889907837 url=www.lawfareblog.com/livestream-house-oversight-committee-holds-hearing-government-coronavirus-response
INFO:root:rank=7 pagerank=0.0639820471405983 url=www.lawfareblog.com/paper-hearing-experts-debate-digital-contact-tracing-and-coronavirus-privacy-concerns
INFO:root:rank=8 pagerank=0.06065132096409798 url=www.lawfareblog.com/israeli-emergency-regulations-location-tracking-coronavirus-carriers
INFO:root:rank=9 pagerank=0.04382758587598801 url=www.lawfareblog.com/china-responds-coronavirus-iron-grip-information-flow
```

**Part 3:**


`globalization`
```
$ python3 pagerank.py --data=./lawfareblog.csv.gz --filter_ratio=0.2 --personalization_vector_query='globalization' --search_query='-globalization'
INFO:gensim.models.keyedvectors:precomputing L2-norms of word weight vectors
INFO:root:rank=0 pagerank=0.7656266689300537 url=www.lawfareblog.com/iphone-huawei-new-geopolitics-technology
INFO:root:rank=1 pagerank=0.14193479716777802 url=www.lawfareblog.com/what-are-implications-emerging-technologies-ai-driven-robotics-and-automation-globalization
INFO:root:rank=2 pagerank=0.08515597879886627 url=www.lawfareblog.com/lawfare-podcast-kate-klonick-and-alina-polyakova-pandemics-platform-governance-and-geopolitics
INFO:root:rank=3 pagerank=0.06749788671731949 url=www.lawfareblog.com/federalism-middle-east-collection-essays
INFO:root:rank=4 pagerank=0.06447222828865051 url=www.lawfareblog.com/federalism-israel-palestine-fresh-ideas-old-problems
INFO:root:rank=5 pagerank=0.06174767389893532 url=www.lawfareblog.com/red-sea-geopolitics-six-plotlines-watch
INFO:root:rank=6 pagerank=0.061579786241054535 url=www.lawfareblog.com/china-pakistan-axis-asias-new-geopolitics-andrew-small
INFO:root:rank=7 pagerank=0.061579786241054535 url=www.lawfareblog.com/waxman-national-security-federalism-bellinger-and-padmanabhan-detention-and-challenges-geneva
INFO:root:rank=8 pagerank=0.061579786241054535 url=www.lawfareblog.com/three-thoughts-refugee-resettlement-federalism
INFO:root:rank=9 pagerank=0.061579786241054535 url=www.lawfareblog.com/our-counterterrorist-federalism
```

`japan`
```
$ python3 pagerank.py --data=./lawfareblog.csv.gz --filter_ratio=0.2 --personalization_vector_query='japan' --search_query='japan'
INFO:gensim.models.keyedvectors:precomputing L2-norms of word weight vectors
INFO:root:rank=0 pagerank=0.10677780210971832 url=www.lawfareblog.com/donald-trumps-statements-putinrussiafake-news-media
INFO:root:rank=1 pagerank=0.10440744459629059 url=www.lawfareblog.com/how-read-news-story-about-investigation-eight-tips-who-saying-what
INFO:root:rank=2 pagerank=0.1030484214425087 url=www.lawfareblog.com/what-are-juggalos-and-why-are-they-marching-against-fbi
INFO:root:rank=3 pagerank=0.0990440770983696 url=www.lawfareblog.com/news-flash-executive-order-detention
INFO:root:rank=4 pagerank=0.09904405474662781 url=www.lawfareblog.com/icymi-former-attorney-general-dick-thornburgh-urges-republicans-defend-mueller-investigation-and
INFO:root:rank=5 pagerank=0.02091512642800808 url=www.lawfareblog.com/germanys-bold-gambit-prevent-online-hate-crimes-and-fake-news-takes-effect     
INFO:root:rank=6 pagerank=0.02042916975915432 url=www.lawfareblog.com/bloody-nose-strategy-self-defense-and-international-law-view-japan
INFO:root:rank=7 pagerank=0.02015472948551178 url=www.lawfareblog.com/japans-evolving-position-use-force-collective-self-defense
INFO:root:rank=8 pagerank=0.019250409677624702 url=www.lawfareblog.com/korea-and-japan-clash-over-history-and-law
INFO:root:rank=9 pagerank=0.018930310383439064 url=www.lawfareblog.com/european-union-steps-its-fight-against-fake-news
```
`apple`
```
$ python3 pagerank.py --data=./lawfareblog.csv.gz --filter_ratio=0.2 --personalization_vector_query='apple' --search_query='apple'
INFO:gensim.models.keyedvectors:precomputing L2-norms of word weight vectors
INFO:root:rank=0 pagerank=0.09854940325021744 url=www.lawfareblog.com/news-flash-executive-order-detention
INFO:root:rank=1 pagerank=0.07414988428354263 url=www.lawfareblog.com/cyberlaw-podcast-mistrusting-google
INFO:root:rank=2 pagerank=0.054828204214572906 url=www.lawfareblog.com/if-facebook-and-google-are-state-actors-whats-next-content-moderation
INFO:root:rank=3 pagerank=0.054828204214572906 url=www.lawfareblog.com/no-facebook-and-google-are-not-state-actors
INFO:root:rank=4 pagerank=0.052541542798280716 url=www.lawfareblog.com/are-facebook-and-google-state-actors
INFO:root:rank=5 pagerank=0.04274430125951767 url=www.lawfareblog.com/bits-and-bytes-labmd-and-google-ai
INFO:root:rank=6 pagerank=0.026314685121178627 url=www.lawfareblog.com/are-facebook-and-google-state-actors-reply-alan-rozenshtein
INFO:root:rank=7 pagerank=0.023560142144560814 url=www.lawfareblog.com/primer-microsoft-ireland-supreme-courts-extraterritorial-warrant-case
INFO:root:rank=8 pagerank=0.022329341620206833 url=www.lawfareblog.com/reactions-microsoft-warrant-case
INFO:root:rank=9 pagerank=0.020840076729655266 url=www.lawfareblog.com/second-circuit-oral-argument-microsoft-ireland-case-overview
```


