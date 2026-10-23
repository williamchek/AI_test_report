# The $50 Hoax: How Fake Statistics Conned the AI Citation Machine

**A researcher invented 50,000 statistics out of thin air. ChatGPT spent seven months treating them as real.**

In March 2026, Metehan Yesilyurt, a researcher at Peec AI, a company that sells analytics for AI search, bought a brand-new domain and set GPT-4.1 loose on it. In a single day the model generated 50,000 statistics pages: SEO benchmarks, content-marketing figures, social media numbers, e-commerce data, web technology stats, all purportedly covering 200 countries. "Every one of them was fake," Yesilyurt wrote. "I never checked a single one."

The bill came to about $30. He later added 15,000 listicles for another $20. All in, the experiment cost roughly $50.

Seven months later, Yesilyurt published his findings on the Peec blog. Google had buried the site within weeks of launch. ChatGPT was still citing it.

That asymmetry is the whole story. Two search systems looked at the same pile of fabricated numbers. One had spent twenty years learning to distrust it. The other had no immune system at all.

## The experiment

Yesilyurt's method was deliberately crude, and that is the point. He bought stateglobe.com, fed GPT-4.1 what he describes as "nothing more than a list of countries and topics plus two sentences," used the cheapest model available, and pushed the output live through a basic AI website template. He then used indexer tools to get nearly the entire site into Google's index within a week.

It worked, briefly. The pages started surfacing in Google results. Then two Google updates landed right after launch, and, in Yesilyurt's words, the site's visibility "collapsed." For months afterward its indexed pages bounced between 5 and 20 impressions. "Google has forgotten the site exists," he wrote.

ChatGPT forgot nothing. According to Cloudflare logs Yesilyurt published, the site received more than 600,000 bot requests in the final 30 days of the experiment. Around 72,000 came from ChatGPT-User, the agent ChatGPT dispatches to open pages in real time while answering someone. "Each one," Yesilyurt wrote, "is ChatGPT fetching a fake statistics page to answer a real question." These were not training crawls. They were live retrievals triggered by real users asking real questions. Google Analytics recorded more than 10,000 ChatGPT sessions since March.

A note on the numbers, because this piece is about exactly this kind of thing: every traffic, bot, and backlink figure comes from Yesilyurt's own analytics, published with screenshots. None has been independently verified. And 10,000 is sessions, not citations. Yesilyurt never claimed a count of how many ChatGPT answers quoted his pages. What he documented instead is arguably more damning: the machine kept coming back for seven months.

## Google forgot. ChatGPT didn't. Why?

Yesilyurt's one-line diagnosis: "Retrieval is not ranking."

A traditional search engine and an AI answer engine do different jobs with the same web. Google ranks a relatively stable index, armored by two decades of spam warfare: link analysis, authority signals, manual penalties, algorithmic updates that can erase a site overnight. ChatGPT, by contrast, rewrites your question into a fan of sub-queries, pulls candidates from search sources and its own index cache, reranks them, and opens a handful of pages to synthesize an answer. "A page can be invisible on Google," Yesilyurt wrote, "and still perform well in every one of those LLM steps."

ChatGPT even runs its own index now. Peec's own research has documented it; roughly half of ChatGPT's turns are served from that index, half trigger live fetches. Either way, the fabricated pages were eligible at every stage, because no stage was asking the one question that mattered: is this true?

## Citation laundering

The ugliest finding is not that a chatbot read fake numbers. It is what happened next.

Ahrefs and Semrush report that stateglobe.com accumulated links from more than 330 domains, including Shopify, DHL, and Bluehost. Yesilyurt does not say what kind of pages those links sit on, and it would be a mistake to read them as editorial endorsements. The likelier mechanism, supported by a follow-up experiment Yesilyurt ran with real, checked data, is prosaic: websites that use AI to write their content cited AI-generated statistics. Machines citing machines.

Four downstream citations were traced and independently verified. A peer-reviewed paper cited the site's "650 million" figure. Qatar's largest financial group, QNB, put its "68%" into a daily market report. India's Business Standard ran the "65% voice search" number. A Canadian influencer community's research brief listed StateGlobe as a source.

Yesilyurt coined a term for this: **citation laundering**. "On day one, a fake number on StateGlobe had zero authority. It was a random stat on a brand-new domain. Then an academic paper cited it. Then a bank. Now the next person, or the next model, that finds this number sees a stat with a source. And that source is cited by serious institutions. The fake data certainly didn't become true, but every citation made it look more trustworthy."

Only citations that linked back could be counted, he notes. "Presumably, many more used the data without a link." What we can see is, by his own admission, the floor, not the ceiling.

## Why statistics are the perfect bait

Yesilyurt breaks the mechanism into four layers, each one favoring the forger. They are his account of his own experiment, not peer-reviewed findings, but they rhyme with what researchers already know about AI retrieval.

First, the query brings its own bait. ChatGPT rewrites prompts into fanout sub-queries and decorates them with modifiers like "statistics," "data," "report," and the year. That is precisely the shape of a stats page.

Second, a number is the most useful thing a synthesizer can find. It is quotable, compact, and slots neatly into an answer. "A specific number is the most useful thing it can find," he writes.

Third, nobody checks. "The model doesn't check the number," he writes. "Fact-checking doesn't happen anywhere in the process." Verification costs latency and money; synthesis is optimized for neither.

Fourth, the reranker scores relevance, not truth. "To get around this, LLMs use rerankers that score relevance, but not truth." And stats pages habitually stamp the year into titles, headings, and URLs, feeding the model's recency bias with a freshness signal the forger gets for free.

## An old trick, industrialized

None of this is entirely new. In 2011, xkcd #978 named **citogenesis**: plant a false claim on Wikipedia, watch "reliable" outlets copy it, then cite those outlets back on Wikipedia. The loop was closed by hand.

The state-level version is called LLM grooming. The pro-Kremlin Pravda network, hundreds of sites publishing millions of articles a year, appears designed less to persuade humans than to seed the retrieval and training pipelines of AI systems. NewsGuard's stress tests in 2024 and 2025 found ten chatbots repeating Pravda-sourced falsehoods in roughly a third of prompted answers.

The scale is what changed. What once required a state now costs $50 and an afternoon. And an industry has already formed around the legitimate end of the same trick: generative engine optimization, or GEO, formalized in a 2023 paper from Princeton, Georgia Tech, the Allen Institute for AI, and IIT Delhi, now sold by vendors offering to get your brand cited by AI for around $995 a month. The line between optimization and laundering is thinner than the industry admits.

## Taiwan: Tinder, No Spark Yet

The timing is worth stating up front. Five weeks after this article publishes, Taiwan holds its nine-in-one local elections on November 28. Prosecutors have already begun election-integrity mobilizations listing AI deepfakes as a novel offense category. The laws, on paper, are severe: up to five years for election disinformation, seven for deepfakes, both written into the Election and Recall Act in a 2023 amendment motivated by fears of generative AI.

In practice, the flagship rapid-takedown mechanism — report to police, forensic check, platforms must remove within two days — has never been used. Not once in three years, by the Central Election Commission's own account. The reasons are structural: it only covers the official campaign window (the first AI voice-clone of the 2024 cycle, a fake Ko Wen-je recording, arrived outside it); "sufficient to mislead" has no objective test (police forensics on an alleged Cheng Wen-tsan video came back "unable to determine"); and offshore platforms are out of reach.

Conviction rates tell the rest. An investigation by READr found 13% of 260 political-rumor cases in 2019 resulted in penalties, 11% of election-related cases in 2020. "How is a judge supposed to rule on this?" a Taipei district court judge asked. "The judge can't investigate the case personally."

The case library is instructive. A fabricated poll in December 2023 showed the KMT ticket leading 33.22% to 32.00%; it was eventually prosecuted, but through funding trails and the Anti-Infiltration Act, not by disproving the numbers. A claimed 40,000-strong rally was deflated by photos of empty plastic chairs. A "secret" US$65 billion arms deal, by definition, could not be checked at all.

Which is the pattern that matters: what makes a claim easy or hard to debunk? Ground truth decides. Named sources can be traced; anonymous drops cannot. Public platforms allow crowd-checking; LINE family groups are invisible and nearly impossible to correct from inside. And numbers cut both ways: a specific crowd count can be photographed, while a "secret" figure carries its own immunity.

The honest footnote: no publicly documented case exists yet of an AI chatbot citing fabricated Taiwan election claims. The tinder, however, is stacked. Taiwanese users already invoke Meta AI to fact-check posts on Threads. Tests show 55% of AI answers contain factual or citation errors. And operators have learned to split research, prompting, generation, and distribution across services — refusals, as one report put it, don't accumulate.

There may be no way to stop it. That is precisely why readers need the habit: the next time a "poll shows" or "statistics indicate" crosses your screen before November 28, ask who measured it. It might be the only defense that arrives in time.

## What you can do

You cannot fix the retrieval pipeline. You can change what you do with its output.

**Find the number's owner.** Who measured it? Which agency, which survey, which year, measured of what? A number with no owner is a rumor.

**Search the figure in quotes.** Drop the exact number into a search engine, open the source page, and confirm the figure is actually there and the context hasn't been twisted.

**Treat citations as leads, not proof.** A link proves a statement was published somewhere. It does not prove anyone measured what the statement describes. If you can't confirm it, cut it.

Anthropic's own documentation says it plainly: always validate critical information; Claude shouldn't be treated as a singular source of truth.

## The bill comes due later

Fifty dollars bought seven months of credibility, and the experiment's cheapest finding may also be its most expensive. Yesilyurt's own closing worry is the one nobody can answer: after this much retrieval and citation, the fake pages end up in web crawls, and web crawls end up in training data. "I hope it hasn't," he wrote, of his invented numbers entering the next generation of models. A guess from GPT-4.1 could become something the next model simply knows.

The provenance of a number, as one commentator put it, is now the person who measured it, not the four sites that repeated it. Until our tools learn to ask that question, the laundering continues, one confident citation at a time.
