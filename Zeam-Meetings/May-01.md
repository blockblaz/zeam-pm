# Zeam Weekly call - Meeting Notes and Transcript
# Date: May 01, 2026

## Meeting Overview
**Date:** May 01, 2026
**Recording:**  https://www.youtube.com/watch?v=r62eB191IqY

---
## Meeting Notes

May 1, 2026
Zeam call  - Transcript
00:00:00
 
Gajinder Singh: Hello everyone, welcome to May one zinc cog and uh the focus is again devet for multi-ubnet runs uh with an emphasis on uh stability as our aggregator and uh parallel threads. Uh so we will also talk a little bit about net 5 with shell and other things that will come up. So let's start with Bara.
Parthasarathy Ramanujam: Right. So last week I've been focusing more mostly running uh multiple subnet uh uh devet uh uh came across several issues which I've reported and addressed a few of them on Zim. Um, one of the main thing that was discovered was Ze uh running as an aggregator starts to fall behind because of uh us not uh uh I mean handling the mutex uh correctly. So we've raised that I've been speaking to Gajender uh we have uh an open discussion on how to do it. So Gajinder following up on that if you can review uh the document that or the approach document or architecture document that Zclaw came up with uh and then we can proceed in minor PRs. What I've done is in parallel uh I took whatever uh uh ideas you gave the other day after a discussion and I have a separate branch where I'm implementing them.
 
 
00:01:37
 
Parthasarathy Ramanujam: Uh but I want to test that out separately while we continue to review what ZCloud builds and then we'll figure out what is probably better. Uh uh I just thought I'd take that approach because uh uh I wasn't confident with some of the responses that Zclaw started to give. So, I wasn't sure if that would uh be accurate enough, but we'll see. I mean, it may probably just be a temporary uh thing. Uh that's one thing on the um I mean uh the mutx issue. The other I found some discrepancies in uh Ze's implementation of uh uh lib P2P uh I mean multi-ubnet thing. It was silently subscribing to all uh the subnets when it shouldn't be. Uh and uh I also noticed uh that um the discovery part we were not paying attention to attestation subnets flag at all. uh while on leanspec this has been uh specified but we on ze side we haven't done so I think there are uh there's one PR that I have that addresses the multiple subscription issue which gender if you can take a look and uh review that would be great uh I would need that before today's run because we are running into issues the second is something I'm working on uh I'll probably uh share as soon as I have a PR ready
 
 
00:03:10
 
Gajinder Singh: So you're talking about PR
Parthasarathy Ramanujam: uh 812 is the one that I want you to take a look.
Gajinder Singh: 812.
Parthasarathy Ramanujam: Yes, that is uh that solves the multiple subnet subscription issue. We we shouldn't be doing that. Um so uh that that is the solution for that. The other is more on the discovery where uh a a client should only subscribe uh or rather only peer with uh uh other clients who are part of its own subnet. Uh I I don't think that has been enforced in Ze till now.
Gajinder Singh: What is not enforcing names? Can you say that part again?
Parthasarathy Ramanujam: So uh in leanspec on the ENR side we have something called AT nets which specifies which subnet a given uh peer is part of it's part of the discovery uh
Gajinder Singh: Amen.
Parthasarathy Ramanujam: announcement but uh in ze uh although we read that we do not uh uh I mean um only preferentially peer with uh a a node which has the same attestation subnet as ours. Um so uh I think that's how beacon nodes uh solve this uh problem of losing right.
 
 
00:04:29
 
Parthasarathy Ramanujam: We don't seem to be doing that.
Gajinder Singh: So I yeah so we need to do that but I think what is right now happening is that we are pairing with pairing with everyone so that is not a problem
Parthasarathy Ramanujam: Correct. So that defeats the purpose. Yeah.
Gajinder Singh: as no pairing with everyone is okay right so as long as you are basically importing everything. Um
Parthasarathy Ramanujam: we at the moment. So we are importing from everyone at the moment.
Gajinder Singh: especially
Parthasarathy Ramanujam: my PR 812 will uh solve that problem. Uh but in the longer run we also need that preferential peering uh thing available. So
Gajinder Singh: so I'm so I thought we had already fixed the issue this particular subscription issue because uh we were only pushing the subnet topics according to we were pushing all the subnet topics uh for ETH P2B instantiation but then when we were actually running the node we were only pushing subnet topics that we wanted. So are you sure that we are still subscribing to all the subnet
 
 
00:05:43
 
Parthasarathy Ramanujam: But uh in ETHP P2B there's a ETH P2P zig there is a pro bug in the code where we subscribe blindly to all topics
Gajinder Singh: topics?
Parthasarathy Ramanujam: rather than only the topics that we should be although we pass that to the library it was ignoring that and subscribing to everything. So that is the bug that has been fixed in my PL
Gajinder Singh: So, so what I understand from what was happening was that we create all these topic list so that uh I don't know whether we subscribe to mamm or not that is something that happens in the rust code we were
Parthasarathy Ramanujam: Yeah. The
Gajinder Singh: creating this topic list to start to init uh the rust bridge page
Parthasarathy Ramanujam: last
Gajinder Singh: but uh we were not subscribing to all those topics because there was in in the node.zig Zig itself. There was this
Parthasarathy Ramanujam: No, you're right. your the the zig part of the code was only subscribing to what it needs to but
Gajinder Singh: particular
Parthasarathy Ramanujam: on the rust glue uh lip p2p glue we were ignoring that uh and subscribing by default to all the topics so that the part no so
 
 
00:07:05
 
Gajinder Singh: but in your in your PR I don't see the rest lip P2B glue changes anyway.
Parthasarathy Ramanujam: if you so uh no no what I'm trying to say
Gajinder Singh: There are no rest li changes.
Parthasarathy Ramanujam: is if you look at it I pass the uh the topics that we need to p to subscribe to right that was not being done earlier. So if you look at key iterator I mean line I think 11 198 on the new uh change uh on ETH lib P2P.zig
Gajinder Singh: Yeah. So I have topic iterator. Okay. Then it appends to the topic list which is
Parthasarathy Ramanujam: Sick.
Gajinder Singh: fine. And then topic list. So that's that's what I'm saying. I don't know whether top so what I think what we were earlier doing was basically we were starting the network first right and I'm not sure whether this topic list
Parthasarathy Ramanujam: Connect.
Gajinder Singh: that uh was being prepared over here was basically subscribed to or it was just used for the definition of lip tob network I'm not sure what what was happening at lip2bite but you are saying that this topic list was all of the topic list were
 
 
00:08:24
 
Parthasarathy Ramanujam: Yes,
Gajinder Singh: substractive
Parthasarathy Ramanujam: that's right.
Gajinder Singh: because then there was a separate code in node.j which was actually doing uh the selection. So maybe we we'll I'll just look at it and then we'll coordinate it off the on
Parthasarathy Ramanujam: Okay.
Gajinder Singh: this and uh what other things that you brought up.
Parthasarathy Ramanujam: So the other uh is uh the mutx related thing. If you can look at uh uh I mean the PR803 which is
Gajinder Singh: So I I I looked at period 8004 I think docs one
Parthasarathy Ramanujam: it.
Gajinder Singh: right 803.
Parthasarathy Ramanujam: Yeah.
Gajinder Singh: Yes. So I looked at it but is there something specific that you want to talk about because I mean
Parthasarathy Ramanujam: No, no. Are you happy with the approach that Zclass uh provided?
Gajinder Singh: more.
Parthasarathy Ramanujam: Uh is there anything anything you need to change there? I did uh comment on a few things, but if you can chime in there too, that would be good.
Gajinder Singh: So on a high level basically I scripted through it and it was fine again testing that basically delegating it to you to look into it.
 
 
00:09:38
 
Gajinder Singh: So can you just tell me the tldr of what you commented over there because that is
Parthasarathy Ramanujam: So uh it had missed a certain conditions uh where uh we might
Gajinder Singh: easier.
Parthasarathy Ramanujam: have to handle mutics differently which it has addressed right now. Uh which I think it broadly agrees with what you had originally asked for but uh yeah I can continue to re review after it starts to generate the code. So, uh, I'll do that. Um, it shouldn't be a
Gajinder Singh: Yeah. So let's I mean it looked fine to me and again I will take another look at it but
Parthasarathy Ramanujam: problem.
Gajinder Singh: let's uh start picking up few of the things low hanging things like borrowed stair or like that and start merging them one by one and because
Parthasarathy Ramanujam: Okay.
Gajinder Singh: then uh once when we do things one by one basically we we can do a deeper dive into what's actually happening.
Parthasarathy Ramanujam: Sounds good. Yeah,
Gajinder Singh: Yep.
Parthasarathy Ramanujam: that that's my status.
Gajinder Singh: So, do do we have a follow PRS on 4803 that have actually done
 
 
00:10:43
 
Parthasarathy Ramanujam: Nothing. Uh no, I haven't.
Gajinder Singh: something?
Parthasarathy Ramanujam: Uh I uh I'll uh get Zclaw to build that and then I'll I'll let you know. Uh my priority today was to resolve the uh P lib P2P stuff so that we
Gajinder Singh: Okay. So, let's merge let's merge this 804
Parthasarathy Ramanujam: can
Gajinder Singh: PR. It's P 8004 but I don't know why it has mentioned 8003 which I
Parthasarathy Ramanujam: Are you happy with that?
Gajinder Singh: will correct him.
Parthasarathy Ramanujam: I thought I I wasn't sure. Uh okay.
Gajinder Singh: I mean I was mostly okay with
Parthasarathy Ramanujam: Okay.
Gajinder Singh: it.
Parthasarathy Ramanujam: Uh that's fine. I'll update the uh it to the latest one and I'll uh do and if you can confirm uh your views on 817 probably we can merge that along with this and together not sorry not 817 813 812 two the gossip
Gajinder Singh: It run.
Parthasarathy Ramanujam: sub mesh uh the one that I described earlier
Gajinder Singh: Yeah, I'll need to check because I need to check what is what was already happening because in my assumption that uh we were already I mean all the topics were being listed for the definition of for lip P2B but we were not subscribing everything because the actual subscription was happening in not zigg and uh so I'll check it and we'll basically talk offline about it.
 
 
00:12:17
 
Gajinder Singh: Uh so in the in this list that Rupur has presented are there any
Parthasarathy Ramanujam: Shut up.
Gajinder Singh: topics that you are mentioned in? So can you take a look and tell me the status of
Parthasarathy Ramanujam: Uh now I think all of these have already been handled.
Gajinder Singh: closure?
Parthasarathy Ramanujam: Nothing else yet. So uh I'm waiting for your uh approval of uh the DevNet for PR on lean quick start. Um everything else seems okay.
Gajinder Singh: Got it. Yes, I have to look into that. All right. Uh let's move to
Katya Riazantseva: Uh yeah,
Gajinder Singh: Katya.
Katya Riazantseva: hello everyone. So um I updated graphana to 13 uh like our um link graphana. I haven't updated yet in the link quick start. So uh I just wonder um para if you're going to merge this PR because this PR definite 4 looks like already very big.
Parthasarathy Ramanujam: It is.
Katya Riazantseva: So probably uh so probably I'd better update Graphana after it's been merged because it's
Parthasarathy Ramanujam: Yeah.
 
 
00:13:29
 
Katya Riazantseva: not like critical change or something just to make things easier. So I can just wait until um it's being merged. Um uh then the metric tick interval has has been merged into ZIM. So currently I'm adding into the specs. Uh also uh yesterday Partha asked for for a new gossip metric. Um yeah which will I open the draft PR as well in the specs uh after we discuss some details today. Um and I continue working with the bot uh which seems very glitchy to me. So it sometimes makes very stupid steps. Uh so I just struggle with it teaching them controlling them how it works how it uh notifies and u yeah that's that's a bit of struggle with the metric and I also as I'm kind of new to alerting system in graphana so I also dive deeper into all these part how to set them up and uh um then I go back to to the bot and try to change it and so back and forth so this is what I'm working now and while Partha is working on the DevNet for running side.
 
 
00:14:55
 
Katya Riazantseva: So I work on the alerts currently and I wonder this uh tick interval metric um was it helpful? Um has it been resolved all the issues we we recorded?
Parthasarathy Ramanujam: So it was definitely helpful but it's not that uh I mean only based on that we discovered the issue wherein um when Zam is running as an aggregator we start to fall behind especially with uh a couple of slot intervals just sorry prior to the aggregation one uh so it's a very useful metric and uh I think it'll be helpful if we were to upstream this to other clients in general to the lean spec itself.
Katya Riazantseva: Okay. Okay. It will be added.
Parthasarathy Ramanujam: Sure,
Katya Riazantseva: I will provide it. Okay. And the gossip metric part we will discuss the detail after the call. Right.
Parthasarathy Ramanujam: sounds good.
Katya Riazantseva: So like labels and all these small parts. Yep. Thank you. that upon me.
Gajinder Singh: Um just listening to the conversation one thing we should do as an optimization uh I don't know bar you or does who does that is basically aggressively or optimistically aggregating uh the payloads if you know that you are already uh the proposer So even before uh the slot interval starts uh
 
 
00:16:23
 
Anshal Shukla: Yeah, I understand it. I'll do it. Uh I think I had a discussion about it with FA as well,
Gajinder Singh: yeah
Anshal Shukla: but uh I didn't have time to do it. I'll uh pick it up soon like in
Gajinder Singh: so basically we have this particular threshold in which we say that okay you know uh for the same
Anshal Shukla: a
Gajinder Singh: attestation data we if we have for example 10 payloads then we'll just aggregate it when we see the 10th one come in. So I think that that is sort of a good mechanism to not do aggregation all the time but also to have that okay you know when our fan out is outside this particular window then we just aggregate it
Anshal Shukla: Right. Uh yeah, I'll pick it up. Uh I'll discuss it with you if I have some confus confusion async, but I'll pick it up.
Katya Riazantseva: Yeah. And the
Gajinder Singh: uh at least let's uh issue all of it uh nour one thing we should also do is basically you know uh the transcript that this generated over here we should basically feed it to Z clause automatically so that let's say when I say that let's create an issue out of it it can basically do All
 
 
00:17:40
 
Anshal Shukla: I think This
Noopur Singh: Okay.
Katya Riazantseva: And the last note for me, I just yesterday noticed that uh in the testing group we had threads.
Gajinder Singh: right.
Katya Riazantseva: So I redirected the graphana alerts to to this graphana alert thread. So they should be there while testing.
Gajinder Singh: So, so these graphana alerts they are coming in the open claw test group or in some other
Katya Riazantseva: Yeah,
Gajinder Singh: group.
Katya Riazantseva: open call testing group graphana alerts thread. Um,
Gajinder Singh: Right.
Katya Riazantseva: so at least currently the restart should be visible and I'm working on
Gajinder Singh: Right.
Katya Riazantseva: the health of of the devet.
Gajinder Singh: Awesome. And I think what we can also do is basically have a continuous analysis of the logs by different clients at any particular sorry
Katya Riazantseva: Yeah, that's that's what I'm doing now. So, but because um there's some issue with alerts themselves.
Gajinder Singh: to
Katya Riazantseva: So I just trying to um set them up properly and uh it started spamming with all of the investigations and logs because alerts were continued like firing and I just need to u brush up the alerts themselves.
 
 
00:19:02
 
Katya Riazantseva: So that's what I'm doing now. So uh so it won't uh like kind because the deet is running now. So head is growing and the finalization stopped. So it just continues, you know, to spell with the alerts. So yeah, that's what I'm brushing up now.
Gajinder Singh: Right. Yeah. So when these sort of events happen basically then it should automatically trigger an an analysis from uh Zclass to tell us what is happening in
Katya Riazantseva: Yeah. Yeah. That's that's the expected behavior.
Gajinder Singh: the
Katya Riazantseva: Yeah. I'm working on this. Exactly.
Gajinder Singh: all right so in this list do we have anything for katya that is not discussed
Noopur Singh: No. Uh uh this graphana version
Gajinder Singh: Do you think that there
Noopur Singh: update has been done
Gajinder Singh: is
Katya Riazantseva: Uh yeah, but uh not on the link quick start but on the um
Noopur Singh: now.
Katya Riazantseva: um link observability. So uh updating Grafana version to 13 on link quick start should follow after the DevNet 4 merge.
 
 
00:20:18
 
Katya Riazantseva: That's that's it.
Noopur Singh: Okay.
Katya Riazantseva: the the last question I have like I've noticed that on on uh on the ex post um I guess our weekly updates are generated automatically and I've noticed that uh for example for my site it was um post it posted that I'm working on devet two and three which is kind of delayed so I don't know if if someone looks into this or not just just note Nothing serious but yeah I mean yeah yeah
Gajinder Singh: You may know the
Katya Riazantseva: yeah.
Gajinder Singh: Twitter.
Noopur Singh: Okay, I will check.
Katya Riazantseva: Okay, thank you.
Gajinder Singh: All right. Uh, okay. Let's move to Angel.
Anshal Shukla: Yeah. So, uh during the last week like I spent majority of the time upgrading the dependencies and the uh Zam version. I I had achieved like quite success in that but later on like there were some uh mergers that were made and there were conflicts I had to resolve them. uh during the last two days I wasn't able toend much time but I uh hope to do it like uh today itself and I have like a local version of running so I'll just review it because majority of it was done using AI so I'll review it and uh raise a PR like update my PR uh apart from that like I finalized my other Merkel cache PR where we have to uh cache the nodes and mark the ones that have been modified as dirty.
 
 
00:22:03
 
Anshal Shukla: So yeah, I did uh raise that PR I think early this week around Monday or Tuesday along with the test cases that I was talking about last week. So uh I had tagged you and GM Gajinder but uh I I haven't received a uh review yet. So as soon as that is done, I'll uh I'll merge it because it is like mostly easy to handle because it has been already upgraded to uh zigg 0.16. So it can be easily upgraded. There are no API changes as such. There's one thing uh that we need to like that I had mentioned in my PR description as well. But it is like in hash tree uh root function we take in the uh we take in the type of hashing that we want to do. So hasher is an input but uh since I'm doing like a merkel caching here so I have initialized Merkel caching by default with SH 256. So either I can like have a array of Merkel caches where for each different type of hasher type I store all these nodes or I can uh add in support for other hashes as well.
 
 
00:23:17
 
Anshal Shukla: Uh or we can uh while initing the uh list or bit list itself we can store like the type of hashing that we want to support but that didn't seem like a good idea to me. So I have right now like the default caching behavior will only work for SH 256 which I think is good for now. We can modify it later
Gajinder Singh: Yeah.
Anshal Shukla: on.
Gajinder Singh: Um I mean if it is basically you know if you take care of the fact that the caching only comes into play when it start off with physics and doesn't come into play automatically I think that is
Anshal Shukla: Yeah.
Gajinder Singh: fine.
Anshal Shukla: Yeah. So that's one thing that
Gajinder Singh: This this is on this is on ssz.z right?
Anshal Shukla: Yep. Yeah. So,
Gajinder Singh: Okay,
Anshal Shukla: uh I I'll reshare the PR if uh if you lost it like I had shared it on the Zoom group. So, maybe like we have had Yeah.
Gajinder Singh: I I found it over there. So, it's all good.
 
 
00:24:10
 
Anshal Shukla: Okay. Yeah.
Gajinder Singh: I'll look into this particular
Anshal Shukla: Uh apart from that like I still need to look
Gajinder Singh: PR.
Anshal Shukla: into the mutex issues that we have faced during this week. Uh there has been some uh so my uh my uh uh parallelization PR got merged. So I think it has it was mostly working fine but it had some minor issues which part Partha resolved. So we are good on that front. I uh I am looking at a few things in the zik thread pool that I have written worker pool that I have written and uh as soon as like I'm fully satisfied with it I'll uh I'll uh shift the ownership to blog
Gajinder Singh: So uh this particular uh PR that we me and
Anshal Shukla: glass
Gajinder Singh: Pa discussed and I think can you take a look at it and Kai has also taken a look at it and I think he has also commented it for Z-class to change few things if you can take a look and uh if you basically need to get some changes done over there because it sort of highlights the high level strategy of again all the things that we are trying to do with regard to mutx and parallel
 
 
00:25:21
 
Anshal Shukla: Yeah. Yeah. Okay,
Gajinder Singh: execution.
Anshal Shukla: I'll do that. I I'll take a look. I I didn't get a uh get the time to look at it yesterday. I think it was posted yesterday or later day before that. So, uh I didn't get time to look into it.
Gajinder Singh: Yeah, just take a look at it and whatever changes you need to do basically discuss with patha and if you guys agree then do it because you already know at high level what I want. I can take a look at it uh as well but it might be delayed.
Anshal Shukla: Yeah.
Gajinder Singh: Uh so decide on it if it looks good merge it and if there are any changes we can anyway resolve it later as well. So as soon as long as we have a consensus on this particular strategy, let's just execute it and not wait too much on
Anshal Shukla: Yeah.
Parthasarathy Ramanujam: Sounds
Anshal Shukla: But there's just one thing that I would like to regress a little bit upon.
Gajinder Singh: it.
 
 
00:26:15
 
Anshal Shukla: So if I'm able to do my if I am able to finalize my zig 0.16 then I would say that we should first merge that because it creates a lot of uh merge conflicts and then I have to retest each and every uh different simulation test and uh beam commands
Gajinder Singh: So this is this is just a doc.
Anshal Shukla: to oh okay
Gajinder Singh: So review it merge it. So there is there will be no code conflicts.
Anshal Shukla: Okay.
Gajinder Singh: This is just a high level doc of what our plan is.
Anshal Shukla: Okay.
Gajinder Singh: Uh but yes there will be follow PR. So if you want to merge your 0.16 PR then we need to do that before we
Anshal Shukla: Yeah.
Gajinder Singh: merge all the PRs.
Anshal Shukla: And try to get it done today.
Parthasarathy Ramanujam: That was one.
Gajinder Singh: So I still need to look at and merge one of the path of PRF but but that is something that your is on SSD
Anshal Shukla: And minuses
Gajinder Singh: right.
Anshal Shukla: Yeah.
Gajinder Singh: Yeah because uh GM will also look at it.
 
 
00:27:14
 
Gajinder Singh: So I will anyway give a
Anshal Shukla: H okay.
Gajinder Singh: review. Cool. uh on DevNet 5.
Anshal Shukla: Okay.
Gajinder Singh: Do we have any
Anshal Shukla: So, uh like on top of what we discussed on Wednesday,
Gajinder Singh: update?
Anshal Shukla: uh not much but I think it has been established that we can deconstruct the proofs. Uh again, Emile had shared uh the APIs, but I couldn't look at it because I wasn't working yesterday. So, I'll I'll take a look and positively reply to it by tomorrow.
Gajinder Singh: Got it.
Anshal Shukla: Yeah.
Gajinder Singh: Uh so on devet 5 I will also take a look at goldfish and see
Parthasarathy Ramanujam: What?
Gajinder Singh: basically you know how to integrate it but most likely it is again as I mentioned in the call that to calculate the target right
Anshal Shukla: Yeah. Yes. So, I understand like the main uh goal behind it.
Gajinder Singh: Oh,
Anshal Shukla: Uh I'll uh So, do you have like a timeline in mind about it? So, I I saw your message about raising a respect for that.
 
 
00:28:17
 
Anshal Shukla: So, maybe I'll do it by Wednesday. This Wednesday. So, uh I think that should doable.
Gajinder Singh: yeah. That is fine. Yeah, brother.
Parthasarathy Ramanujam: So, uh I am uh I think ML merged DevNet 4 on lean multi yesterday. So, I think we'll have to update our uh hash sorry hash glue to the latest commit on DevNet 4. Uh I think there was some improvement on AVX. Uh uh so yeah if you can do that or or I can do that too and if you are held up with other things let me
Anshal Shukla: So I'll do it. It I think it's a minor change on Zoom side.
Parthasarathy Ramanujam: know.
Anshal Shukla: It's a uh I'll have to rec uh recompile the binaries for the uh Python specs and push it push them and update it on the inspect. So I'll do that as well.
Parthasarathy Ramanujam: Okay sounds good. Uh while we are discussing that I raised this with no earlier gender earlier uh if I were to ask Zclaw to review a PR it used to approve it on GitHub as well now I think the permission is gone.
 
 
00:29:24
 
Parthasarathy Ramanujam: I was just wondering if we can probably add some limited permissions so that if uh if there is a PR that doesn't require or doesn't involve a consist consensus change or anything like that but we needed for DevNet for restarts I could probably ask Zclaw to build it and I could approve and merge it. So is that something we are agreeable to and providing access to Zclaw for PR approvals?
Gajinder Singh: uh I mean I'm not sure whether what kind of fine grain control we have that we can give to Z-class because if Z-class can approve it it can approve any P
Parthasarathy Ramanujam: Right. But can we agree within the team that we only uh merge a PR that
Noopur Singh: Yes.
Parthasarathy Ramanujam: Zclaw is approved if it doesn't affect uh so and so uh modules like if there is nothing related to consensus it's just uh other bug fixes is that something we can merge so that we don't uh we I mean there's no hard dependency on you to approve a UDZ as
Gajinder Singh: So I I think uh in we can do it on basis of tag.
 
 
00:30:34
 
Gajinder Singh: If we can have some tag on the PR that the PR author has put
Parthasarathy Ramanujam: Right.
Gajinder Singh: and uh then it could automatically show approval maybe something like that.
Parthasarathy Ramanujam: Okay.
Gajinder Singh: If see if we can do
Noopur Singh: Okay. So like release PR I can I can ask that it can uh auto approve and part type you
Gajinder Singh: that
Noopur Singh: can just like we can decide on some uh PR tables.
Parthasarathy Ramanujam: Yeah, something like lip p2p or uh chore or something of that sort. Anything that involves consensus or some major change uh I think we should avoid
Noopur Singh: So,
Parthasarathy Ramanujam: uh it from having approval
Gajinder Singh: I mean we we can we we should just do it on the basis of tag and leave it to the judgment of the PR
Parthasarathy Ramanujam: to
Gajinder Singh: creator to put the appropriate tag.
Parthasarathy Ramanujam: Okay.
Gajinder Singh: Uh because I mean because we can always have a rever uh reverse review of what the PR creator did, right?
Parthasarathy Ramanujam: Correct.
Gajinder Singh: So since the team is trusted so in that sense
 
 
00:31:32
 
Parthasarathy Ramanujam: Okay.
Gajinder Singh: as long as uh you put the tag and uh we are trusting you to put the correct tag. So Zclaw should be able to approve if you can have that sort of a approval
Parthasarathy Ramanujam: Okay.
Gajinder Singh: process.
Parthasarathy Ramanujam: Yeah,
Gajinder Singh: No
Parthasarathy Ramanujam: sounds good.
Noopur Singh: Okay, I'll check. I'll ask to make that
Gajinder Singh: or I mean we don't we might not even need that.
Noopur Singh: change.
Gajinder Singh: We can just say that okay since he asked for Z clause approval then that is good enough
Anshal Shukla: So only uh the team members can ask for Zclass approval,
Gajinder Singh: right
Anshal Shukla: right?
Gajinder Singh: I think
Anshal Shukla: Yes. So in that case I think we can just rely upon the members
Gajinder Singh: Oh.
Parthasarathy Ramanujam: But who who are the team members who have
Anshal Shukla: themselves tag uh per say
Noopur Singh: Oh,
Parthasarathy Ramanujam: access?
Anshal Shukla: like I can put release tag on my consensus PR just to get the Z clause approval and get it merged.
Noopur Singh: water.
 
 
00:32:36
 
Gajinder Singh: Yeah.
Anshal Shukla: So
Gajinder Singh: Yeah. It is just to trigger it so that it reviews and approves it.
Anshal Shukla: anyways,
Gajinder Singh: But yes, we are trusting the team as of now.
Parthasarathy Ramanujam: Yeah.
Gajinder Singh: So if I think I don't think we do we need any label as Anchel has mentioned. If you just simply request from Z clause and it approves it and you think that it won't affect
Parthasarathy Ramanujam: Come on.
Gajinder Singh: consensus then it's good because I think uh we have we have spent uh decent amount of of time on Zam now together as a team and sort of uh we understand what is to be touched and what is not to be touched without a review.
Parthasarathy Ramanujam: Yep, sounds
Noopur Singh: Okay. So,
Parthasarathy Ramanujam: good.
Noopur Singh: part what you can do is like just ask uh Zcloud to like review and approve if uh it is acceptable. So,
Parthasarathy Ramanujam: Uh yeah,
Noopur Singh: if
Parthasarathy Ramanujam: I tried that yesterday. It says it says that it's uh instruction in tools.mmd or agent.mmd does not allow it to approve a PR only provide review command.
 
 
00:33:45
 
Noopur Singh: Okay. Okay, I'll check that out.
Gajinder Singh: So you can actually see in uh blog blaz/zclause repo
Parthasarathy Ramanujam: Yeah.
Gajinder Singh: whether there is such an instruction because whatever it uh so we have instructed zclause to push whatever changes it has configured in this particular repo. So if it says it has something like that then it should be over there as well. If you look into blog blaz zclaw repo all the configuration for zclauses should be
Anshal Shukla: Yeah, also like I I think like instead of relying upon
Gajinder Singh: over
Parthasarathy Ramanujam: What's
Anshal Shukla: Zclass for approval and stuff,
Parthasarathy Ramanujam: so
Gajinder Singh: there.
Anshal Shukla: maybe we can give the team members the merge uh merge uh permission so that they can do it if they deem it to be a minor change and not a very conflicting one.
Gajinder Singh: Yeah. So we we can basically also configure that in zclaw behavior that uh it can also sort of take this call but obviously it's an AI and it's intelligence is yet to be proved in the real sense right so intelligence is of the level of a junior engineer
 
 
00:34:56
 
Parthasarathy Ramanujam: Yeah.
Gajinder Singh: so we are saying that okay junior engineer make a call whether you think this is good or not sometimes it behaves like a decent engineer but sometimes is not so I don't know okay I mean yeah let's for now basically say that okay uh let's try to program this into zcloud behavior itself that if it thinks that it's a minor change then it can directly approve
Noopur Singh: Okay, that I'll make the change.
Gajinder Singh: Cool. Anything else we want to discuss anchel from these points that are mentioned over there? Is there anything that is pending?
Anshal Shukla: uh so yeah refactoring of libex CV is pending uh so I was I had proposed that I'll remove the is schedule uh parameter from the functions and uh refactor mock doz in such a way that all all of them can push to a particular queue and read from there itself but that is pending uh I'll try to do it this week itself but yeah I I'll eventually handle it it's not I think I it's not a blocking change so uh I'll but I'll keep make sure that I'll eventually handle it
 
 
00:36:33
 
Gajinder Singh: Yeah. Yeah. I just wanted to get an update on the points we last discussed so that we nothing slips
Anshal Shukla: maybe
Gajinder Singh: through.
Anshal Shukla: Yeah.
Gajinder Singh: Do we have anything for Kai over there?
Anshal Shukla: Yeah.
Gajinder Singh: Even though he's not there, was there anything pending on Kai? in this particular list. No code. Is there anything that is pending on
Noopur Singh: I'm not sure.
Gajinder Singh: Kai?
Noopur Singh: I do not have the updates from Kai.
Gajinder Singh: But we have Kai share a region log.
Anshal Shukla: So okay yeah I think stationation tolerance has been handled.
Gajinder Singh: Okay. Dis.
Anshal Shukla: It has been handled on the lean spite as well. uh logs
Gajinder Singh: But we also needed to handle it in terms of whatever is the newest behavior of adding the tolerance
Anshal Shukla: test.
Gajinder Singh: for an interval, right?
Anshal Shukla: Yeah, that I don't think is done right now.
Gajinder Singh: because Kai did mention that something like that on the channel itself. One other thing we would like like is that Zclass will basically also uh you know bring up the points that are being mentioned in the channel uh that we are chatting in the channel that okay you know we'll talk about this or this needs to be done so that it if it
 
 
00:37:57
 
Gajinder Singh: can also create those points list for us to discuss in this particular call. So if we can have sort of a total 360deree integration now I'm doing a manager speak but uh yes what we did is a 360 integration into everything that is happening and give us a unified view
Noopur Singh: Okay. Okay. I'll try to uh do that.
Gajinder Singh: right cool anything else we have to discuss Okay, my update is that I have been looking at PRs and looking at uh
Parthasarathy Ramanujam: Oh,
Gajinder Singh: DevNet 6 uh proposal as well as DevNet 5 uh consensus proposal. Um yeah,
Parthasarathy Ramanujam: that's Uh I had two okay I had two
Gajinder Singh: that is my update.
Parthasarathy Ramanujam: questions one uh we've seen a few external contributors submitting uh PRs uh over the past week. One of them I believe is a replacement to the CLA flags which is a breaking change. Uh I think he's handled both after I spoke to him on both lean quick start as well as Zim. But I don't want it to be merged right now uh because we are having other PRs that are ongoing.
 
 
00:39:19
 
Parthasarathy Ramanujam: So uh I mean I I just prefix the PR with do not merge. The other I believe is uh uh I mean uh a same PR with deals with the mutex. So I was wondering if we can have some u first time issue related stuff that we could create for new contributors or anything of that sort otherwise they may pick uh stuff that we are already working on and uh it may discourage them in future. So uh and nu did say there's nothing available right away but uh just something for us to consider u maybe in the near future. Uh that one that's one thing. The other uh more was a question of do we have plans to replace the h lib P2P glue with a pure zig implementation that Kai was working on and is that something we can give priority to because uh this foreign function interface with rust and zig is already started to becoming painful and uh I'm sure we'll run into more issues uh when we release I mean new devets going forward.
Gajinder Singh: Yeah, I don't know about uh the priority because priority also means that do we have money for it, right?
 
 
00:40:38
 
Gajinder Singh: And I'm not sure and uh I'm not sure Kai and others
Parthasarathy Ramanujam: Okay.
Gajinder Singh: have taken it to some logical conclusion. Right? So maybe when Kai is here next time we can ask this question because we do wanted to change to it or should we just try to put some uh C library right? I don't know.
Anshal Shukla: No, but eventually we have plans to move to ETH lip uh ETH P2P, right?
Parthasarathy Ramanujam: I mean it's P2P.
Anshal Shukla: So if we Yeah, ETH P2P. If we move it to lip P2P then we'll again migrate to ETH P2P then it's
Parthasarathy Ramanujam: Yeah.
Anshal Shukla: like anyways if beacon chain is not going to use it then I don't think what is how will it be used in like the ZG ecosystem in general left
Parthasarathy Ramanujam: Right. So I I have a working version of uh Zigg implementation of ETH P2P but uh
Anshal Shukla: side
Parthasarathy Ramanujam: what is the uh I mean timeline for beacon switching over to ETH P2P is that something that's agreed
Gajinder Singh: No, I don't think uh there is anything in agreement.
 
 
00:41:48
 
Gajinder Singh: It is totally in research phase as of now.
Anshal Shukla: also like we'll have to anyways maintain the FFI at least for the forthcoming
Gajinder Singh: So,
Anshal Shukla: future in in terms of like uh one is like hash that I think path you had worked upon and another is like multi-lean multisc so for these two things we'll have to
Parthasarathy Ramanujam: Hey.
Anshal Shukla: continue using them so I think it's okay to just stay stick to that maybe we can think about it later
Parthasarathy Ramanujam: Okay. I mean my only concern is that we are not taking
Gajinder Singh: Yeah, is uh is Yeah,
Parthasarathy Ramanujam: the no I was just saying we are not taking full
Gajinder Singh: go
Parthasarathy Ramanujam: advantage of zig uh when we are relying on uh external um
Gajinder Singh: ahead.
Parthasarathy Ramanujam: libraries that was my only concern. uh so we don't get the the exact performance benefit we would in a pure zig program like um say e lambda or other uh rust based clients are able to achieve
Gajinder Singh: Yeah, I mean my resolution would be just to throw more hardware over it, right?
 
 
00:43:00
 
Gajinder Singh: Have an additional core or whatever additional and deal with it
Parthasarathy Ramanujam: Okay.
Gajinder Singh: rather than us spending dev cycles on something that is sort of tangential as of now. uh but yes I mean I I do agree that uh
Parthasarathy Ramanujam: Mhm.
Gajinder Singh: a native implementation will be good but again we don't have resources to spend on
Anshal Shukla: Yeah, because if we a native implement,
Parthasarathy Ramanujam: Okay.
Gajinder Singh: it
Anshal Shukla: we'll have to debug it here and there and eventually we'll swap it out. So like working something and then eventually throwing it away. So rather work on what's present on later do a better version when things get fin more agreed upon across the ecosystem.
Gajinder Singh: And my integration of lip P2P was I mean based on some hacky way of instantiating it because uh I mean uh because as I was having some issues running it on the main thread and not get and getting it locked. So maybe there are better ways of doing the bridge. So if someone wants to look into that.
 
 
00:44:24
 
Gajinder Singh: So that part is something that we could make better because I I I I'll just confess that I didn't have too much uh understanding of Tokyo and all that right so whatever runtime it runs on and whether took is the best runtime or they we should run it on standard runtime or what kind of threads we use. So I have I had very less inside of it but I just made it work somehow resolving the issues and we just went from over there and kept on adding layers onto it. So if there is a better way to do do a bridge yes we should do that. All right. Uh, anything else we have on the table to
Noopur Singh: Um so I just wanted to mention that I have upgraded uh uh upgraded
Gajinder Singh: discuss
Noopur Singh: open claw and after that it has it seems to have become more secure and it uh like it broke what a lot of things were working earlier and uh but uh also mentioned that it is hallucinating all the more now. So we'll have to uh just keep an eye out that we do that we like we we take we do regress like double checks and
 
 
00:45:49
 
Gajinder Singh: Awesome.
Noopur Singh: all.
Gajinder Singh: So yes, let's keep an eye on it and uh see whatever whatever we need to reconfigure it. Uh and now Kai has joined us. Kai, do you want to give us give us update?
Kai Chen: Uh yes, I just uh want to you we review my new PR. So I think I need both to merge.
Gajinder Singh: which is the PR
Kai Chen: So uh y is uh 755 uh
Gajinder Singh: number.
Kai Chen: 754. So yeah this uh changed to uh follow the the new spike change.
Gajinder Singh: Yeah, we were talking about this a bit earlier and I was remembering that yes, you have done some work on it. So, I'll just review it and then we
Kai Chen: Yes. And the and the Yes. If uh and another is the spike
Gajinder Singh: can
Kai Chen: test but must uh merge list before spike test or there is a lint issue. Yeah. So I have
Gajinder Singh: What is that? What is that?
Kai Chen: uh uh
 
 
00:47:39
 
Gajinder Singh: PR number.
Kai Chen: 715 that that had um uh several new kinds of spike
Gajinder Singh: Yep.
Kai Chen: fast.
Gajinder Singh: Got it. So I will take a look into both.
Kai Chen: Yeah.
Gajinder Singh: But your 7:15 PR is yeah CI is not passed because you are
Kai Chen: Yeah.
Gajinder Singh: saying that it's baking on
Kai Chen: Yeah.
Gajinder Singh: lend
Kai Chen: Yeah. Uh, I will fix the link
Gajinder Singh: all right and uh there is para can you share your
Kai Chen: is
Gajinder Singh: PR uh number to kai which in which you have tried to change the topic subscription
Parthasarathy Ramanujam: Uh yeah 812 I I shared that uh I think he said that he wanted you or Anel to have a look at
Gajinder Singh: Yeah. So, I'll have a look at it.
Parthasarathy Ramanujam: it.
Gajinder Singh: But Kai, can you also review it?
Kai Chen: Oh yes, but uh I remember you you and an so current current uh approach I uh I remember you and an so agree this uh before. So I'm I'm not
 
 
00:49:18
 
Parthasarathy Ramanujam: uh no kai there were two things uh one that we Anel and I were discussing was
Kai Chen: sure
Parthasarathy Ramanujam: more on um uh the mutx related uh thing this particular PR is for multi-subnet subscription there was uh uh What we noticed was that uh ETH uh P lib P2P.zig was subscribing to all subnets even though uh the client uh is configured to run only on one subnet. Uh so uh the PR812 addresses that uh
Kai Chen: Yeah.
Parthasarathy Ramanujam: issue
Kai Chen: Yeah. Yes. I know. But but I remember Gajenda um seems and anious seems uh Luke had this list before and they they want they want it.
Gajinder Singh: Yeah.
Kai Chen: But maybe I'm wrong.
Gajinder Singh: So, so we did look at it and I was under the impression that uh you know when we are editing then all the topic list that we are preparing is for the definition of lip tob uh but we do an actual subscription in node.zig.
Kai Chen: Yeah.
Gajinder Singh: So this is what my current understanding is that is what we are doing.
 
 
00:50:32
 
Gajinder Singh: But
Anshal Shukla: Oh yeah, I had also looked into it. So we were specifically adding on the subnet uh
Gajinder Singh: uh
Anshal Shukla: ID where we were specifying that it should not subscribe to these particular subnets.
Parthasarathy Ramanujam: Yeah, but that is applicable only for aggregators,
Anshal Shukla: So
Parthasarathy Ramanujam: right? You configure subnets only if the role is an aggregator. For a regular tester, uh you don't have that uh configuration. So what happens is the client automatically subscribes to all the
Gajinder Singh: No, no.
Anshal Shukla: Okay.
Parthasarathy Ramanujam: subnets.
Gajinder Singh: So that is something that uh that I need to check what is happening in the current PR in the current code because my understanding is that all these topics that we are preparing for anything they are they're not subscribing they are just creating lip definitions and then uh we are subscribing actually later in node.zig Zig when we are actually running uh the node uh so I'll just take a look at it and Anel and Kai you can also take a look at it but yes what we do want is that we don't want to subscribe to those subnets where we are not aggregator or not uh publishing any attestations right so we definitely don't want to subscribe to that those subnets so if that is happening then we need to correct that.
 
 
00:52:03
 
Gajinder Singh: So let's all review what current code is and what path PS proposed and see you know
Kai Chen: Okay.
Gajinder Singh: if the new pair makes sense or whatever else we need to do to fix it. Anything else Kai you are working on or plan to work on next
Kai Chen: uh current currently not but uh
Gajinder Singh: week?
Kai Chen: but for the mutex uh design maybe uh I'm not sure though. So I have some uh consider consideration to to make make it simple. So so because I I saw we have so we we have too too many mutex right now. Maybe I want I want to
Gajinder Singh: So two too many too many mutxes are not a problem but I mean the way our understanding of that should be simple that okay this is a resource which is bound by mutx and this is the way it works. uh so I'm we don't I don't really want to say that okay we need less mutx we want maximum parallel execution and we want uh understanding of mutx in the
Kai Chen: Yes.
 
 
00:53:34
 
Gajinder Singh: sense that we know that okay what is borrowed and what is not
Kai Chen: Yes. But yeah, but so what what functions can be parallel? So maybe we we need we need more cons consideration. So maybe uh several functions that they that can not be benefit
Gajinder Singh: So that is what I designed.
Kai Chen: for
Gajinder Singh: I basically added in the document. Right? So in my eyes only uh most of the functions can be parallel right. So uh because when we finally do changes in some particular resource right either it's in fork choice or in the caches that we have right so those can independently uh be parallel independently independently mutated. Uh so only thing where this cannot happen and we need a multi-resource lock is when we are doing pruning because uh uh because when we get canonicality view from the fork choice we don't want changes in other resources till we prune because it changes everything. So at that is the only point in my eyes that things basically you know uh needs to be atomically locked across uh across multiple resources but otherwise things should be maximally uh parallel and we should basically take advantage of this particular behavior of sik and in our mind also we should be aware that okay things could be parallel.
 
 
00:55:13
 
Gajinder Singh: So if we need some snapshot we should have snapshotting capabilities that okay you know we we have taken snapshot and for example in serving request response we should just take a quick fj snapshot and just release it and then independently serve it and not hold uh the mutx any mutx over there right so uh so this this is what my thinking is and it is sort of documented over there and I have also seen that you have commented uh and ask Zclass to make some changes over there. So I'll again review the doc document and see what where we are. So are you on board with that particular document? So can you also further review and uh add your comments on
Kai Chen: Uh yes.
Gajinder Singh: it?
Kai Chen: Yes. But um but I think the most most import um most uh important one is uh we should we should not uh if uh even though we need parallel we should uh avoid in the rustly P2P thread and uh leave XEV main thread. So this these two thread
 
 
00:56:40
 
Gajinder Singh: So why why do you think that we should avoid industry P2B thread?
Kai Chen: um
Gajinder Singh: It is just a thread. Is it any different or does it block P2P?
Kai Chen: I think yeah so I think that that red is for the maybe the the main thread for the
Gajinder Singh: Hey,
Kai Chen: to Tokyo SC schedule. So maybe it's a event loop thread. It didn't need handle the multiple IO um async functions.
Gajinder Singh: But but lip2b should already be multi-threaded right so I'm not sure whether it should give us
Kai Chen: So
Gajinder Singh: on the multiple main thread because if it's giving then we need to fix it on the P2B level so that we know that my understanding is that it should not be doing it on multiple mainet h but again I
Kai Chen: So for me I so for me I I think we we
Gajinder Singh: I have very limited understanding of uh rest parallelization routines
Kai Chen: should avoid any heavy business logic in the iOS Well, it's
Gajinder Singh: But I mean what I see in other clients is that they do all these things on the main thread right
 
 
00:57:57
 
Kai Chen: almost
Gajinder Singh: on the whatever is the uh you know gossip for example whatever is the gossip serving thread. So they do those things on the gossip service or they are having this particular thing where they are just taking the objects and then pushing it in some of the thread.
Kai Chen: So
Gajinder Singh: So can you for example take a look at lighthouse whether what we are saying is actually
Kai Chen: that's
Gajinder Singh: true or they are just using that particular thread. My understanding is just a thread right? So it should not stop lip loop but again this is just a conjecture needs to be
Kai Chen: Yes. And and uh beside beside uh beside the rustly pp in M
Gajinder Singh: verified.
Kai Chen: because we use lib XCV. So that that thread is also uh need need nonblocking. If if it's broken, it will impact the next slot. Um because you use the timer. I think that may be delayed next time
Gajinder Singh: So firing of the clock I think it's not really.
Kai Chen: once.
Gajinder Singh: So you're saying that we should also not be doing it on licv thread and then starting another threads to do actual processing.
Kai Chen: Okay.
Gajinder Singh: Okay. So can you guys also basically look into Anchel Pa can you also guys look into this execution model of libex CV and
Parthasarathy Ramanujam: Sure.
Gajinder Singh: uh anything if you can make of rustly P2B because maybe our fundamental assumptions are wrong but it doesn't seem to be but let's be more clear about that. All right. Uh cool. So let's do all these things that uh we have discussed and again the aim is to make Zim as the most robust client out there. We are not there yet but uh let's do it. Thank you everyone. See you next week on 8th May.
Anshal Shukla: Thanks to
Parthasarathy Ramanujam: Bye.
Gajinder Singh: All right. Have a good one.
Kai Chen: See you.
Noopur Singh: Thank you.
 
 
Transcription ended after 01:00:55

This editable transcript was computer generated and might contain errors. People can also change the text after it was created.
