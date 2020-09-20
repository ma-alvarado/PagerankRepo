
In this project for my data mining course, I created a simple search engine for the website https://www.lawfareblog.com .
This website provides legal analysis on US national security issues.


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
Using the `--search_query` feature, which takes a string as a parameter, the program returns all urls that match the query string sorted according to their pagerank.
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

In the case above, we have removed the blog's most popular article (www.lawfareblog.com/snowden-revelations).
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
Using the `--search_query` option, on the other hand, will see classify "important" websites as those deemed so by ANY other website.

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

`japan`:
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
`apple`:
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