# Zeam Weekly call - Meeting Notes and Transcript
# Date: April 24, 2026

## Meeting Overview
**Date:** April 24, 2026

---
## Meeting Notes

Apr 24, 2026
Zeam call  - Transcript
00:00:00
 
Gajinder Singh: Hello everyone, welcome to April 24 Zoom call. Here we'll discuss DevNet 4 ongoing runs and the issues that we are facing over it as well as our DevNet 5 proposals if anyone has to add any if anyone wants to add anything to it. But uh before that Nupor will take us through uh what we discussed in the last call.
Noopur Singh: Uh hi, this is um this is a list from the last call that uh was supposed to be followed up on and uh most of these were on you Anel. Um so um I can check if uh like Anel and Kajenda did you coordinate amongst you to for the uh for the pruning PR the block pruning PR?
Anshal Shukla: Yeah. So I can go over through the list uh regarding my portions.
Noopur Singh: Yeah. Yeah. That's
Anshal Shukla: Yeah. So it was uh not really like the block pruning PR. It was related to the race condition we were uh facing in the between the network and the main loop. Uh and there was some fixing this by para as well.
 
 
00:01:20
 
Anshal Shukla: So yeah, I did share the PR and Pa had like uh merged my PR into his PR and then his PR got merged. So this issue has been uh resolved. I think Nepur also checked it on uh uh her machine and she confirmed that it uh solves the issue that uh Gajinda you were also saying in the Linux machines. So yeah, I I did do that. uh about like the second uh point about refactoring the lib XCV I haven't done that but I looked into the another PR that has been raised on uh top of uh raised on theme by one of the first time contributors so it uh I have looked into like how we can do it and I think like we can refactor mog.zig zig in a way. Uh maybe we can just uh separate it out as a separate uh separated uh separate simulation test package where we can have a Q sort of a thing where which can uh where we can push these events RPC calls and it every other uh nodes in the simulation test can uh consume it.
 
 
00:02:28
 
Anshal Shukla: So I think it makes sense to refactor it in such a way. It doesn't make sense to like have this convulated structure just for the sake of uh mocking. So I think I can uh I think I can raise a PR on this in a couple of days with the refactor. So I'll do that. Uh about like the reviewer testations. Yeah. So I did had this discussion with Gajender and uh we also discussed it in the PQ interop call on Wednesday. So uh yeah that is also done. Apart from that like I raised another PR regarding like parallelization of verification and aggregation uh compaction. So yeah that PR I raised yesterday. So maybe I can get some reviews there.
Gajinder Singh: ing got it. So I'll take a look at the parallelization PR also. uh whether we are taking care of it already because it was in some PR by partha where with the gossip objects we should also start caching their bite code because we have to clone them or to uh basically especially for the block to serialize them uh so we don't need to do that so that bad code is already available so if we basically can if we can carry some cached information and uh uh which basically might also include uh computing uh the block hash uh and things like that.
 
 
00:04:11
 
Gajinder Singh: So so instead of just having uh having just the block structure we we carry on some of these cast information to optimize downstream. I think that would also help. I'm not sure what's the status on that.
Anshal Shukla: I think that also got merged alongside my uh my PR. So that was the PR in which like Python merged my PR and then eventually I approved it and it got merged.
Gajinder Singh: Okay. So, okay, I'll take a look at the recent code and see how it's happening and see if uh there is something additional that needs to be done over
Anshal Shukla: Yeah. Yeah.
Gajinder Singh: there.
Anshal Shukla: And also like I uh in this particular PR uh I had like a wrong understanding about block pruning occurring which was basically uh causing the sec fault due to like the shadow shallow copy of the uh pointer. it was b uh basically because of the rehashing of the hashmaps which was causing the index to change and uh the uh and I was passing it across the across the threads via the index.
 
 
00:05:21
 
Anshal Shukla: So basically due to due to like increase in the hashmap there was like rehashing of the indexes and I was pointing out to uh pointing out to a particular index which wasn't really initialized. Uh yeah, apart from that like during the last week I was involved quite a lot in uh reviewing the PRs as well. So like that's it from my
Gajinder Singh: Also I have another question that has popped into mind.
Anshal Shukla: end.
Gajinder Singh: What have we done with regard to uh caching the hashtree route and sort of applying updates on this? We wanted to do that but I'm not sure where we are on it.
Anshal Shukla: Yeah. So uh I have done that. I haven't raised the PR yet because I was thinking of adding a few test cases before I am like uh before I re raise the PR. So that is also in uh process. I hope to do it like today or tomorrow but uh yeah it is already in my mind. I'll I'll do that. Yeah, but uh anyways like uh since uh GM has already upgraded the
 
 
00:06:31
 
Gajinder Singh: Great.
Anshal Shukla: zig version on SS library, we won't be able to merge it right away. Uh I'll see if I get some time and if there aren't much changes, I'll try to upgrade the Z version on Zam as
Gajinder Singh: Yeah,
Anshal Shukla: well.
Gajinder Singh: I think if uh we can we should just upgrade the version on Z as well.
Parthasarathy Ramanujam: Yeah,
Gajinder Singh: And with regard to that,
Parthasarathy Ramanujam: I thought No, no,
Gajinder Singh: yeah.
Parthasarathy Ramanujam: sorry. I thought GM was saying that he's going to do that as u earlier today, right? Uh, I'm not sure if he already has a
Gajinder Singh: Okay. Okay. So let GM put in a PR for this.
Parthasarathy Ramanujam: PR
Gajinder Singh: I guess he was Yeah, he mentioned that he was uh trying to move trying to test out Z16 as
Anshal Shukla: al so I think I had looked into that last week and there were some of the downstream libraries
Gajinder Singh: well.
Anshal Shukla: that didn't support it and that's why I didn't do it like the XCV library itself liV library and apart from that like other libraries that we use are pretty small and can easily be upgraded and most of them I have already forked onto uh our blog plus organization for uh for zigg 0.15 so not sure how he's tackling
 
 
00:07:56
 
Gajinder Singh: I mean um so does Zigg not allow basically compiling the supply liaries with their own compilers? I mean what I'm assuming is that what should generally happen is that uh if you have all the zinc compilers available in your path it should basically pick the current one for compiling uh whatever downstream upstream you're using downstream.
Anshal Shukla: Uh, not
Gajinder Singh: Yeah,
Anshal Shukla: sure.
Gajinder Singh: maybe Z has might not have reached to that level of sophistication as of now.
Anshal Shukla: I'll I'll check, but I don't know.
Gajinder Singh: Right. Yeah. Just checking if this is the physibility then basically it unlocks. All right. This is your update over or do you have more? Okay. All right. Let's move to Kai.
Kai Chen: Uh yes. Uh so for the test station five or Yeah. Uh I already update uh some comments in the PR. I suspect uh it's it's about clock screw uh issues. So maybe uh other uh some other clients already uh already enter slots uh slot n but uh but see uh still in slot and minus uh minus one.
 
 
00:10:02
 
Kai Chen: So we need get some adaptation from that client.
Gajinder Singh: So did you address the comment that I made?
Kai Chen: Uh
Gajinder Singh: So basically yes in clock skew it is possible that we might get uh at a stations but like I don't think this much would be the clock because at a station you would get an interval after interval two right interval 0 1 you would get after interval one and I I I don't think this much would be clock skew
Kai Chen: So, so I just so because uh current in current uh spike the block and attestation uh both tolerate uh slots plus one. So other client or or or uh or implement this. So right now we maybe we we are the only
Gajinder Singh: Yeah, but I still still don't get why why should that estation slot be
Kai Chen: one
Gajinder Singh: in future because if it's interval if that estation is created interval I by that time I'm assuming that clock skew won't be more than 250 millisecond all across the world uh but is clock skew across 250 mcond And the thing that you observed I mean interval right now is like 4x5 which is like 800 millcond right so my assumption is that people's clock would not skew more than
 
 
00:11:51
 
Kai Chen: Yeah,
Gajinder Singh: 250 millisecond but it is also possible that their clock ticked earlier so it's 2 * 250 mcond which is 500 millisecond maximum.
Kai Chen: heat.
Gajinder Singh: minimum skew which still does not make sense that our will not have tech by the time.
Kai Chen: Is it is it possible because because uh right now we uh we
Gajinder Singh: So do you do you have the log for this?
Kai Chen: treat
Gajinder Singh: Because right now in our log we attach the current slot and current interval we are right now.
Kai Chen: um
Gajinder Singh: Uh so if so if that is mismatching then we clearly know that interval then the slot is not matching.
Kai Chen: So I think it's uh only only and check the zoom log maybe not not enough to to catch this uh cases. We
Gajinder Singh: So this is what we are trying to solve for the zam node right. So in the zen log you should be able you should have this right when you for example threw the feature slot error then our log will have the slot and interval on our local z node.
 
 
00:13:17
 
Gajinder Singh: So do you have that log because then it would basically tell us clearly whether this is the case that you mentioned that there was a clock skew because we so we use whatever local fork choice uh slot and interval is when we log it out and that is the reason why I added it to get these kind of clarity.
Kai Chen: Yeah. Yes, I think yes.
Gajinder Singh: Do you buy the log of this particular
Kai Chen: Yes. I think uh we uh I have this log.
Gajinder Singh: app?
Kai Chen: Yes. That log uh raise raise a lot of uh attestation too far.
Gajinder Singh: So can do you do you have the log in the PR? Can you mention the log in the PR so I can take a look?
Kai Chen: Yeah. So I think the log already in the uh issue opened by uh Katilla.
Gajinder Singh: which is the
Kai Chen: So,
Gajinder Singh: issue
Kai Chen: uh let me see.
Gajinder Singh: because I don't see that issue opened by Zclaw on our main channel.
 
 
00:14:40
 
Kai Chen: Uh yes, I think it's opened by
Gajinder Singh: So,
Kai Chen: Is
Katya Riazantseva: I think it was opened by Zo but uh it's related to on PR
Kai Chen: that
Gajinder Singh: so do you know what ization
Kai Chen: okay? So,
Gajinder Singh: number.
Kai Chen: I don't see Is it
Noopur Singh: It's 603.
Anshal Shukla: Just
Kai Chen: closed?
Gajinder Singh: What is the issue
Anshal Shukla: I think it's 7:52.
Gajinder Singh: number? This is about using publishing.
Kai Chen: What?
Gajinder Singh: I don't know this is
Katya Riazantseva: Yeah, it's it's 7:52.
Gajinder Singh: different.
Katya Riazantseva: Two.
Kai Chen: Yeah.
Gajinder Singh: No, if we are producing restations then how is how how how is the interval not updated because the way we run intervals is uh we first run interval on node and then that we run interval on the validator. So there is no way validator would call publish produced testation aggregation without the interval being ticked. So this is not possible if the interval is not syncing question. Then the problem is something else. I mean it's guaranteed that if we are producing something at some interval we would be at the interval and also I don't see the interval log over here.
 
 
00:16:48
 
Gajinder Singh: I mean I don't see that the way we produce the log.
Parthasarathy Ramanujam: Uh I I think I've seen this error during the uh I mean um a quite aggressive uh pruning that we did because it the given source block was not part of our case and it it came up with this. I'm not sure if that fixed this
Gajinder Singh: Yes.
Parthasarathy Ramanujam: issue.
Gajinder Singh: So this is unknown source block issue is different from the interval take issue right. So this is not even that issue.
Parthasarathy Ramanujam: Yeah, I I don't think
Gajinder Singh: So I so question is what what is that issue that we
Parthasarathy Ramanujam: it's
Gajinder Singh: are trying to solve over here and which which is your PR sky what is your PR number
Parthasarathy Ramanujam: I think it's 754.
Kai Chen: Yes. Yes.
Gajinder Singh: is also 75
Kai Chen: I think there there are there are some error
Gajinder Singh: Yeah.
Kai Chen: about attestation to too far in the in the log which uh KIA attached.
Gajinder Singh: So there's the
Kai Chen: Yes.
Gajinder Singh: log
 
 
00:18:07
 
Kai Chen: In in the issues 752 there there are there are several kind of arrows in that logs.
Gajinder Singh: I only see one particular error over there. Where where is where is error? I don't see the feature. Okay. In totz. Okay.
Katya Riazantseva: So this this issue was about other issue like unknown source block but Kai found uh from this log like kind of another issue but it didn't mentioned in this specific issue.
Gajinder Singh: So if you basically take any log we have slot an interval attached to it. I'm just just from that log I'm just copying something onto our chat. So if you see that whenever we start a log we have slot is equal to Z I is equal to Z to say what interval we have locally and this is something that you should in the error itself we should be there unless we are not logging the error as you're not doing lo kind of a log future I'm not able to search for future
Kai Chen: Uh, not featur Too far.
 
 
00:19:44
 
Kai Chen: Too
Gajinder Singh: far.
Kai Chen: far.
Gajinder Singh: Okay, it's not taking me anywhere for too far. Okay, I'm not getting it. It was slow too. So, yes, we have warning gossip test station. So it seems like um I don't know it seems a very odd thing because the current
Anshal Shukla: So no it comes
Gajinder Singh: slot is 511 and we are getting the destation for 512 slot.
Anshal Shukla: when
Gajinder Singh: This does not make any sense right.
Anshal Shukla: yeah so there's a limit of one I think mercy had done this change where we have
Gajinder Singh: Oh,
Anshal Shukla: like we reject the attestations from the future slots and the overall uh tolerance is of one slot. So if we re
Gajinder Singh: but why why would we even tolerate one slot over here? Because it's not even that you know if the slot is 512 that means there is 4 second we are 4 second behind from some of the no that I don't think is even possible right we 4 seconds skew is not possible so if you look at it if you look at the log that I pasted in the chat right so we are at slot 511 interval 2 and we received goss testation for slot 512 why would we receive a destination for slot 512. It's not even
 
 
00:21:33
 
Gajinder Singh: possible.
Anshal Shukla: Uh it is yeah we have a a constant variable max future slot tolerance in constants doz we have defined this and
Gajinder Singh: Okay.
Anshal Shukla: Hey,
Gajinder Singh: So if I just take when 512 take it take that. So 18527 it ticked in 1 second. So yeah, it's bit weird that why in 1 second we so intervals we there are three intervals. So at least there should be a diff of two or there's the diff is 1.1 second. Let's say if we were on the end of the interval to over there. So we have third, fourth, fifth, five. Okay. So we need to actually see
Anshal Shukla: So so if the attestation is from the block we allow it. If it is uh a gossip attestation then we don't allow it. But uh I need to look
Gajinder Singh: So, so we allow from for the block because block comes at the block boundary and if you are you have a skew then it is possible that you're you are just at the previous slot and you got a new block right or you have not yet ticked it locally and you got the new
 
 
00:23:40
 
Anshal Shukla: Yeah.
Gajinder Singh: block but for a test station they they are coming in interval two or they you can start seeing as fast as interval one
Anshal Shukla: Okay.
Gajinder Singh: and uh but why would basically use you you be so behind right there's no reason for you to see an adestration belonging to the next slot over here because even if we look at the log so 512 stick that 18 6. So this is this is weird, right? Because the last log we have at interval 2 is at 18.4 and then we see interval and the next slot ticked at 18.3. So at 18.4 sorry 18.4 you are at slot 512 interval 2 and then suddenly at 18.6 Six, you are at slot five and 12, interval zero, which basically means that there is some delay in processing the interval itself. So this is not a problem of interval slot too far. This is a problem of we not being able to process the intervals in the current timeline. This is what it means it I understand why it so can so can we can we see how much time did we take so for each interval taking do we have a metric for it kya how much time
 
 
00:25:34
 
Katya Riazantseva: No, no.
Gajinder Singh: a note took
Katya Riazantseva: Because uh the metrics metrics are more global. Uh what we can do, we can measure and add it into the log like how many uh one atomic operation takes because metrics are like 50 seconds and they are more like medium. So they won't give this that detailed info.
Gajinder Singh: Yeah, they want kind of detail correct but
Katya Riazantseva: It should be better to put in the log.
Gajinder Singh: no but what we can have is uh the diff right so if you have an interval so so if interval one was at this particular time and next interval ticked at some particular time we can we can post of the interval. So we can have a rate of the interval being ticked. So that would basically give us a variation if you know something is taking too long for the node to take the local interval. So maybe this particular metric is not only good for us, maybe it is good for everyone and we can have this as a global metric.
 
 
00:26:40
 
Katya Riazantseva: So rates between intervals like ticks on on every second.
Gajinder Singh: Yes. So, so basically when you take the interval what is what is the time at which you so so it's like uh taking of the interval you plot it against time and then when your local fork choice take the interval so it
Katya Riazantseva: Yeah. Okay, I got
Gajinder Singh: is it is I think it would be a counter right so
Katya Riazantseva: it.
Gajinder Singh: if you plot the rate of the counter then you would get the graph where you would take more time to take the interval locally which basically means that you took longer time to process something. So can we see in this do we have graphana for this particular time so that we can see okay why have we taken so long to take the interval which I don't even understand so all these are test stations from
Katya Riazantseva: Uh, we would
Gajinder Singh: the block but are these test stations from noise gossip test stations uh so even if there are these gossip test stations why has our clock not ticked because clock is running on an independent rail so let's Okay.
 
 
00:27:52
 
Gajinder Singh: So, so there are let's say on 17.9 seconds or so after that why did the interval not take at this particular time? It could also be that the clock is not running nice in a nice manner. We have clock ticking log or not? We don't we don't have clock taking log. I think we have it. But in the debug logs, where are the debug logs for this particular thing? K. Do we have debug logs for this?
Katya Riazantseva: uh in my understanding this is already debug lo because we added it
Gajinder Singh: This is not all in
Katya Riazantseva: before then I can check if uh
Gajinder Singh: logs.
Katya Riazantseva: I think there was zen restart and console lock didn't wasn't safe for Yes.
Gajinder Singh: So who is who's going to look into these debug logs and tell me what exactly happened?
Katya Riazantseva: What we don't have for debug because for every
Gajinder Singh: We should have debug.
Katya Riazantseva: for
Gajinder Singh: This is before we sorry this is before we take take down we
Katya Riazantseva: restart
 
 
00:29:06
 
Gajinder Singh: are able to implement debug locks path. I mean where is what
Parthasarathy Ramanujam: No are available but for this particular run we won't have because they are
Gajinder Singh: time?
Parthasarathy Ramanujam: uh saved on a separate file and which I believe is not copied over to uh graphana
Gajinder Singh: Okay.
Parthasarathy Ramanujam: Loki.
Gajinder Singh: So can we see if we are still seeing this error log and get debug logs for it and troubleshoot this out.
Parthasarathy Ramanujam: Okay, we I'll keep an eye out for this uh issue and if it comes uh again I'll I'll share the logs and
Katya Riazantseva: It's just the this debug log it it um overrides every time
Parthasarathy Ramanujam: it
Katya Riazantseva: we restart the devet. So uh with every new start of Zim uh overrides that the the data folder cleans up.
Parthasarathy Ramanujam: uh I'll probably have to set up a script to roll over those uh instead of overwriting we just take a backup. I'll I'll see what I can do
Katya Riazantseva: Yeah, probably.
Gajinder Singh: and Kazak in the meantime can we add the the metric for taking the interval taking the
 
 
00:30:03
 
Parthasarathy Ramanujam: here.
Gajinder Singh: interval/ slot I mean interval is good enough if there is any clock issues they will directly show up there not us not just for us for everyone it's useful So we should just add this as a global metric.
Katya Riazantseva: Yeah, I will open PR today. So I will try to make it on Z side and check how it
Gajinder Singh: So like like we have it for slot we just do it for interval as
Katya Riazantseva: works.
Gajinder Singh: well.
Katya Riazantseva: Yep.
Gajinder Singh: All right.
Kai Chen: Okay. Okay. Uh by by the way there is there is a spike spec test for checking this behavior. So right now we we failed this spec test.
Gajinder Singh: So can you repeat that Kai with regard to spec
Kai Chen: There is a pet test that check the uh
Gajinder Singh: test?
Kai Chen: tolerance uh current slots uh plus one. So we we we don't have this uh logic. So there there is
Gajinder Singh: So it should be it should be reverted as well.
 
 
00:31:27
 
Kai Chen: a
Gajinder Singh: Why does the gossip test station have tolerance of plus one? Can you raise this in the PQ interrupt channel and uh tag me as well so that I can follow up that why do we have this tolerance? We don't need any tolerance over there in the customer
Kai Chen: Okay.
Gajinder Singh: stations.
Kai Chen: Okay. Uh so for the spec tests uh currently I'm focused on the fork choice uh state transition and the SSD uh yeah fix fix the bug in SS uh library. So right now uh SST spec test all passed. Uh they are only uh so I also I also uh fix two uh add two checks uh in our logic in the PR in the PR 754. Yeah. So there are two spec test check some uh uh uh fer slots uh compare. So we we don't have this. So, so right now there is only one build uh spec test for choice. Uh and uh there are some other other kinds such as uh gossip sub uh so right now I uh I'm not I'm not uh working on that.
 
 
00:33:43
 
Kai Chen: So I want to uh fix the folk choice first.
Gajinder Singh: So lazy proto proto array prune on finalization we already fixed it right because we now don't no we we still rebase the fork
Parthasarathy Ramanujam: Yeah.
Gajinder Singh: tress on we rebase the fortress as well as on the last one or what what did we
Parthasarathy Ramanujam: We prune up to uh the last uh that is the current finalized block minus one.
Gajinder Singh: prune
Parthasarathy Ramanujam: So the last finalized one everything else is retained in cash.
Gajinder Singh: so also we we rebase also to the last one I don't actually one second Let me just
Kai Chen: Anyone?
Gajinder Singh: see. All right. We only ring slot to root cache. We are not we should also basically delay the rebasing. This is what K is second point is K. How have you fixed it? Let me see.
Kai Chen: Which one?
Gajinder Singh: Prune lazy prune of the choice if choice get not.
Kai Chen: Oh.
Gajinder Singh: Okay.
Kai Chen: Oh yeah. Uh I see there is a PR merged
 
 
00:35:35
 
Gajinder Singh: Yeah. Yeah. Yeah. Okay. I I see.
Kai Chen: by
Gajinder Singh: So ba can also can you also review this particular PR because it's also sort of rel related and instead of having the way he has uh the way Kai has fixed can we also do similar over here in terms of using the previous finalize rather than having a rune
Parthasarathy Ramanujam: Sure.
Gajinder Singh: threshold. just share. So I just shared on the Zen channel what section I'm talking about. Just take a look at it. And what else on spectus change? Hang one second. Let me look at the main description. Drop time base block use. No. Why? Why should we not have time based block Q? Okay, I'll check into why why you have so is there a reasoning K you added for not having time base block Q rather than rather than just processing it with a plus one tolerance or this is just for spec test uh purposes.
 
 
00:37:39
 
Kai Chen: Yeah. What?
Gajinder Singh: Okay. All right. So maybe we can have the third point the resolution that you did for the third point. Good night. But I'm not sure before we process the feature block do we do we need to process the text that is my question to process the feature block so I'll check into your solution whether it's also taking the local for fork choice forward as well is it taking the local fork choice forward kai when you process a feature Look
Kai Chen: Hello.
Gajinder Singh: because we also need to see what are the implications for it that we are processing the feature block without taking the local fork choice forward. So we basically need to see that as well. So, I'll I'll take a look at it. What else is on the spec test? Okay.
Kai Chen: Perfect.
Gajinder Singh: Some further validations that you added. All right. So I'll take a look on this and then we can move further on this particular PR.
 
 
00:39:56
 
Gajinder Singh: Anything else Kai you have to you want to
Kai Chen: Okay.
Gajinder Singh: add.
Kai Chen: Uh, no.
Gajinder Singh: Okay. So let's now move to Kya.
Katya Riazantseva: Uh so this week uh DevNet 4 metric PR has been merged uh reviewed by anal. I also touched some metrics from de 3 which which didn't look good. Um so we set up the web hooks for um zo uh but uh currently it doesn't work for me. Uh so we are trying to solve it with nukur. So probably the updates needed for zero in general because uh it returned several. So uh before that before that fail uh I teached him how to notify about restarts but then it failed and start to send messages to nupor. So uh this is how it glitches. Um yeah so uh we should fix that and uh continue monitoring how it works. Um, other than that, this week I was on a big Graphana conference, like big three-day conference here in Spain. They announced uh the new 13th version of Graphana and uh all the updates in it.
 
 
00:41:34
 
Katya Riazantseva: So, um I was following it and u I'm going to update our Graphana and implement some new features that could be useful for us. there's uh regarding visualization syncing with GitHub and others like they have AI assistant as well but I need to check how it works u how can we deal with for example free assistant or if it possible so this is yeah what I'm going to work on um these days that's it thank
Gajinder Singh: What is the issue with the notifications?
Noopur Singh: um like OpenCloud started hallucinating and instead of uh sending it on the GitHub sorry on the uh on the test group it started DMing me and uh like I don't know where it get got the instruction to DM me so that's what we were wondering
Gajinder Singh: So basically, did you not ask it to correct it or it
Noopur Singh: I asked it so it has corrected uh it correct itself now but uh like we don't know the origin of that instructions.
Gajinder Singh: So I mean there could be no region. This is what the systems are for or instead of
 
 
00:43:03
 
Noopur Singh: Yeah.
Gajinder Singh: that.
Noopur Singh: Yeah. So I corrected that and it should depend on the on the group but K is right now getting some
Gajinder Singh: Okay.
Noopur Singh: error from the from open clause. So I will try to fix it today.
Gajinder Singh: So is is uh is the bot or is the open claw container that we are running is able to get the notifications and process it on
Noopur Singh: Yes. Yes, it is uh it is able it is able to get it and process it and send send it to
Gajinder Singh: time.
Noopur Singh: the telegram group. It sent a I think two uh two uh notifications it sent and then it started sending me on telegram. Um but yeah it is processing processing the through the web
Gajinder Singh: And do we also have a some poll process in case
Noopur Singh: hook.
Gajinder Singh: it does not uh receive the notifications for whatever reason.
Noopur Singh: Uh no like we do
Katya Riazantseva: I have a I have a duplicated alert currently because it's still in like test
 
 
00:44:05
 
Noopur Singh: not
Katya Riazantseva: net mode. So it's just uh yeah I'm just trying to follow how it works. So, not only the claw, I don't follow fully um lean on on the claw only.
Gajinder Singh: Right. Now my question is that uh if for example the notification doesn't go through for example you know it's just posting a notification through HTTP and sometimes the those calls can fail for whatever reason. So if those notifications don't go through, does it retry by itself or do we need to pull notifications from somewhere?
Katya Riazantseva: it it can't pull itself. So, uh it can follow
Gajinder Singh: I I mean it by it I mean Grafana.
Katya Riazantseva: um
Gajinder Singh: So I I'm I'm assuming Grafana is the one sending the notifications.
Katya Riazantseva: Yes, but Grafana duplicates it as well. So, it sends it to me. So I can catch this.
Gajinder Singh: Now my question is that if Graphana is not able to deliver the notifications does it retry later on or or does it has this feature later on?
Katya Riazantseva: I I don't know.
 
 
00:45:22
 
Katya Riazantseva: I don't know. I need to check.
Gajinder Singh: Okay. And this is this is what kind of notification. This is just the data
Katya Riazantseva: now just an alert like um for example
Gajinder Singh: dump.
Katya Riazantseva: uh so uh there are different ways to set up notifications but they are mostly based on on the metrics. So for example uh head minus finalize delay or something like this. So based on the metrics it has.
Gajinder Singh: Okay.
Katya Riazantseva: So
Gajinder Singh: So you can configure so you can create the events and these sort of events and uh send the
Katya Riazantseva: yes and after the clause gets the notification for example if it's
Gajinder Singh: notifications.
Katya Riazantseva: like finalization told it technically can do some research using the logs uh loy logs like what happened but as I've previously told uh it should be double checked because currently maybe it will get uh this skill later like doing a lot of investigations. Uh but currently they all need to be double checked.
Gajinder Singh: understood. And uh I think what we should also ask the bot claw is to also keep a track of its learnings in a file so that it can basically develop this knowledge base.
 
 
00:46:59
 
Gajinder Singh: Nupur I think we might need to instruct it to do something like that.
Noopur Singh: Okay. So, uh by learnings we mean the what uh we what what the what the open claw learns through the graphana alerts or the learning that it it should handle it this way. So
Gajinder Singh: I think both.
Noopur Singh: like
Gajinder Singh: So what it is it has learned on what is the status of the network and second that uh if for example it said something and we corrected it then it needs to uh basically learn that particular behavior means that particular
Noopur Singh: Yeah, that that should be auto like the later part it should have it should happen in open flaw automatically because it's supposed to be um self-arning but uh
Gajinder Singh: I think armies was self learning.
Noopur Singh: we'll
Gajinder Singh: I'm not sure whether open close is self learning or
Noopur Singh: like
Gajinder Singh: not.
Noopur Singh: I think it is but I will double
Katya Riazantseva: I I think that yes. Yeah. Like it has its own history and then it can improve its skills.
 
 
00:48:05
 
Noopur Singh: check.
Katya Riazantseva: I've noticed that it refers to for example Zim code base. So it it already knows it. So
Gajinder Singh: All right. Uh okay. No, this is also because we have configured it that way. We have told what its purpose is and so we have this particular skill that it automatically picks up and implements. So it it whatever it configuration files are it basically keeps pushing it onto uh GitHub blog play zclause repo. So we should be able to see whatever it has learned and configured over there. So if for example we have told it whatever changes you make in your configuration you should push it to this particular library this particular repo. All right let's move forward uh to pa
Parthasarathy Ramanujam: Right. Uh last week uh I spent uh time uh debugging the DevNet 4 on the logs ran with uh a single subnet one and uh that discovered few issues whenever Zim was running as an aggregator. We uh addressed few of the points uh after discussing with Kajjinder.
 
 
00:49:42
 
Parthasarathy Ramanujam: So uh now we are not aggressively pruning. So uh it was a lot Ze was a lot more stable. uh managed to run it for close to 18 hours and uh it went on well. Uh uh the network stalled only due to a crash in a different uh client did and not because of Ze. uh but we've did identify a couple of issues which for which one of the PRs uh has been merged this morning. So Gajinder I had a question you mentioned that I have to raise a spec PR but it wasn't clear to me what uh should I be doing because uh the spec
Gajinder Singh: So in this you said that in this pack basically they are merging new
Parthasarathy Ramanujam: is
Gajinder Singh: with uh known to calculate uh uh safe target. So that needs to be corrected on the
Parthasarathy Ramanujam: Okay. So,
Gajinder Singh: spec.
Parthasarathy Ramanujam: uh essentially align spec to be compliant with what Zam is doing right now. Ze is the correct version and uh that that's what you mean correct.
 
 
00:50:42
 
Gajinder Singh: Yes, my yes.
Parthasarathy Ramanujam: Yeah.
Gajinder Singh: So we do this PR as per my understanding Z is the correct version and if it's not then there will be a
Parthasarathy Ramanujam: Okay.
Gajinder Singh: discussion that will happen on the spec PR itself. So we use the spec PR to push forward this discussion and clarification
Parthasarathy Ramanujam: Okay.
Gajinder Singh: because uh as per my understanding that is why we use new not known because if you look at uh the tech processing
Parthasarathy Ramanujam: Okay.
Gajinder Singh: interval on uh 3SF mini so so after you calculate the next the safe target you accept
Parthasarathy Ramanujam: Mhm.
Gajinder Singh: new votes in the next interval. itself, right?
Parthasarathy Ramanujam: Right.
Gajinder Singh: So, so if you and accepting uh the new accepting new votes basically means merging new votes into non and if that was needed to be done, why not do it before calculating safe target?
Parthasarathy Ramanujam: Yep.
Gajinder Singh: So, so that basically is a clear indication that safe target needs to be calculated on the on the new boards which is basically an availability protocol and as I messaged on the channel availability is based upon
 
 
00:51:52
 
Parthasarathy Ramanujam: Mhm.
Gajinder Singh: online validators.
Parthasarathy Ramanujam: validate
Gajinder Singh: online validators basically uh as per your view what what but what what what are the
Parthasarathy Ramanujam: this.
Gajinder Singh: votes that you have saying for this current new slot right
Parthasarathy Ramanujam: Okay.
Gajinder Singh: because each time you consume new you accept new that news get consumed right so that gets empty and then only the next slot uh uh destations will show up in the new so right so once you consume that. So that basically means that nonfestations should not be merged to calculate the safe target and if that
Parthasarathy Ramanujam: Okay.
Gajinder Singh: is happening in the spec we need to raise it as well as discuss it if uh consensus people over there says no this is the correct version because as per my understanding it is to availability protocol runs on online validators and online validators for your particular node means whatever is What are the new test stations that you have seen All
Parthasarathy Ramanujam: Okay. All right. I'll I'll uh after this call I'll go ahead and raise that spec tag.
 
 
00:52:58
 
Gajinder Singh: right.
Parthasarathy Ramanujam: Uh everyone I'll tag you on PQ uh channel. I mean uh yesterday I did raise the issue but there was no other comment apart from um Toma saying that there may be an issue with the spectus. So um I'll I'll let you know once that's done. Um when I ran u a devet with two subnets one of uh the thing that came across is that uh I mean Ze is printing uh an error message saying that a duplicate gossip which is not necessarily an error it's just a information message. So I was I wanted to check with Kai if we can change that to a debug log rather than an error. Uh because if the message hash matches it just reports duplicate uh message rather than just saying that uh this is something that has already been propagated.
Gajinder Singh: But because of this message he figured out that there were the so there were basically duplicate of validator indexes in registered validator ids because of which the there were two attestations which were created for the same index.
Parthasarathy Ramanujam: Right.
 
 
00:54:16
 
Parthasarathy Ramanujam: No,
Gajinder Singh: Right.
Parthasarathy Ramanujam: this actually presented itself.
Gajinder Singh: Mark after that
Parthasarathy Ramanujam: Yeah. Uh sorry.
Gajinder Singh: piece.
Parthasarathy Ramanujam: Uh what I was saying was that when I make an aggregator listen to attestation from multiple subnets only then this error appears not otherwise. So, uh,
Gajinder Singh: But why would uh a validator what is the error?
Parthasarathy Ramanujam: for example,
Gajinder Singh: Can you say that
Parthasarathy Ramanujam: uh, duplicate, um, so, uh, I'll have to look at exactly.
Gajinder Singh: again?
Parthasarathy Ramanujam: I think it says duplicate, uh, message or something.
Katya Riazantseva: It's like gossip message duplicating,
Parthasarathy Ramanujam: Yeah,
Katya Riazantseva: right?
Parthasarathy Ramanujam: gossip duplicate. Gossip message duplicate or something like that.
Gajinder Singh: Okay. So got it. This should already be handled by lipid tob,
Parthasarathy Ramanujam: It is but we are uh it does not propagate the message but we are logging an explicit error
Gajinder Singh: right?
Parthasarathy Ramanujam: saying I mean in zen logs it says error z duplicate gossip message or something like that
Gajinder Singh: But which which is which layer is logging
 
 
00:55:14
 
Parthasarathy Ramanujam: it's actually a warning uh e
Gajinder Singh: it?
Parthasarathy Ramanujam: p2p zig or sorry yeah e lib p2p
Gajinder Singh: Okay. So I think it is coming from lip itself and uh we can basically suppress it to debug.
Parthasarathy Ramanujam: Yeah. Okay.
Gajinder Singh: Can you can you take a look at
Parthasarathy Ramanujam: Uh
Anshal Shukla: But what is the gossip message? It it is like is it like a block message or a attestation
Gajinder Singh: it?
Parthasarathy Ramanujam: uh I think it was on the aggregation uh message.
Anshal Shukla: message?
Parthasarathy Ramanujam: So you're receiving the same or rather you're propagating the same aggregation message twice.
Gajinder Singh: Propagating or receiving? Because propagating means we are
Parthasarathy Ramanujam: Yeah, we are publishing right um or
Gajinder Singh: publishing.
Anshal Shukla: So ideally we should only publish it once on a subnet.
Parthasarathy Ramanujam: um so uh this is across subnets uh isn't it? So should I mean this is where I'm
Gajinder Singh: There is no subnet for aggregator, right?
Parthasarathy Ramanujam: confused
Gajinder Singh: So there is just one channel for aggregator.
 
 
00:56:18
 
Gajinder Singh: There is no subnet for aggregator message. It's global anyway.
Parthasarathy Ramanujam: but when an but when an aggregator uh listens to attestations
Anshal Shukla: Yeah.
Parthasarathy Ramanujam: from multiple subnets it won't be publishing those attestations uh again or would it
Anshal Shukla: Heat.
Gajinder Singh: It it it aggregates and it publishes the aggregated message but there are no subnets in the aggregated message that is that is a global topic. So a destation has subnets.
Parthasarathy Ramanujam: Yeah.
Gajinder Singh: A destation topics has subnets. Not the aggregation aggregator topic has aggregation or whatever is the topic
Parthasarathy Ramanujam: Aggregation. Okay. Okay.
Gajinder Singh: name.
Parthasarathy Ramanujam: I I'll I'll have to uh double check this. I thought it was on the aggregated uh setup and um we shouldn't as what you say maybe this shouldn't
Gajinder Singh: So can you can you post the message on Zoom channel so that we can look
Parthasarathy Ramanujam: occur. I'll I'll do that.
Gajinder Singh: and Yeah.
Parthasarathy Ramanujam: Uh yeah. So my uh uh I mean uh task this week is to get multiple subnets
 
 
00:57:16
 
Gajinder Singh: Okay.
Parthasarathy Ramanujam: stable. Hopefully after the current release of Zim that should be the case. If it goes through then uh I'll go proceed with uh increasing the number of subnets and scaling up even further.
Gajinder Singh: Can you message me your lean quick start module subnet PR?
Parthasarathy Ramanujam: Uh it's the same devet for PR.
Gajinder Singh: I want to take a look at it.
Parthasarathy Ramanujam: I I'll do that. Um I have also identified few improvements that I can do to those. I'll be creating them as separate PS. But this one I'll message you after the SC.
Gajinder Singh: All
Parthasarathy Ramanujam: I think I think you already approved it but there have been a couple of changes after that.
Gajinder Singh: right.
Parthasarathy Ramanujam: So you might have to reapprove.
Gajinder Singh: Okay. I just want to see the subnet assignment uh again and verify that this
Parthasarathy Ramanujam: Sure.
Gajinder Singh: is what was in my mind and what I discussed with kata and atcc k if you can also take a look into that particular PR
 
 
00:58:09
 
Parthasarathy Ramanujam: Okay.
Gajinder Singh: and clarify anything that you need with
Katya Riazantseva: Okay.
Gajinder Singh: para all right uh on uh so my update is that I I've been looking at PRs and looking at uh issues that have been propping up and trying to figure out what's been happening. uh and also a little bit planning on what we can do on DevNet 6 which is basically uh where we try to move closer to beacon layer functionality and uh basically the first step is to integrate the execution and support the validator life cycle. So this is my thought process so that by the end of the year we can have a longunning devet which is public and which people can come and join and exit. Also I have been thinking whether we should do it in a normal uh normal execution integration like it's currently or we should do it in an epvs mode. uh that has been that was another thing that I have been thinking but I I'm not really sure about it whether EPBS is the model that we would want to go at hard for there will be some reset some debt technical debt resetting of that model as well which is something we can I guess discuss
 
 
00:59:55
 
Anshal Shukla: EPB has been approved from for Amsterdam,
Gajinder Singh: Yes,
Anshal Shukla: right?
Gajinder Singh: but it could also it is also possible that uh there is I mean from what I understand research is not really a fan of EPBS EF research and uh they think that there could be a better uh APS model and if that is the case then maybe that is what we should try to integrate Or we can just go with the PBS. I don't
Anshal Shukla: Okay.
Parthasarathy Ramanujam: Okay.
Anshal Shukla: Yeah.
Gajinder Singh: know.
Parthasarathy Ramanujam: You shouldn't relate to the present
Anshal Shukla: Uh apart from that there's one more thing that just photos will like
Parthasarathy Ramanujam: team.
Anshal Shukla: start we'll get back
Parthasarathy Ramanujam: Yeah. Yeah. That's what I meant.
Anshal Shukla: on.
Parthasarathy Ramanujam: Whatever you do, don't tell it to port. That's
Gajinder Singh: Yep. No port will come to know, right? If we propose something he will definitely come to know and obviously if we don't want him to oppose Ethereum we would basically not say that we are kicking out EBBS but uh I mean that is a job just needs to manage so we'll just wave our hands from
 
 
01:01:16
 
Parthasarathy Ramanujam: Yeah.
Gajinder Singh: it.
Anshal Shukla: Yeah, also uh I'll be upgrading the ZM repo and I'm thinking of removing hash 6 CLI completely. It is a dependency because we have like the hasher configurable right now but it creates a dependency on hash 6 CLI which uh I have like updated to Caitana's uh folk of Caitana hash 6. So uh which uh chitan did some updates but I am thinking of removing it completely. We can reintroduce it once we have like a decision on the hashing side as
Gajinder Singh: But H6 CLI is something we just use in lean
Anshal Shukla: well.
Gajinder Singh: quick start. Why do we use it in Zamreo?
Parthasarathy Ramanujam: No, no, I think he's referring to the Zigg implementation, the hashig one that I wrote that Chaitananya has for faked to introduce Prooseidon hashing. uh the work that he was doing
Anshal Shukla: Yeah. Yeah. Yeah.
Parthasarathy Ramanujam: earlier
Gajinder Singh: Okay. So, all right. Well,
Parthasarathy Ramanujam: and uh and Angela I'll take the sorry I'll I'll take a look at that LMDB thing I've assigned that issue
Gajinder Singh: let's
Parthasarathy Ramanujam: to myself
Gajinder Singh: All right guys, I guess that will be all for today's call. We are already running over time. So we'll see you on one May same
Parthasarathy Ramanujam: Yep.
Gajinder Singh: time.
Katya Riazantseva: Right.
Parthasarathy Ramanujam: Thanks.
Anshal Shukla: Okay,
Gajinder Singh: Thank
Anshal Shukla: thanks everybody.
Parthasarathy Ramanujam: Bye now.
Gajinder Singh: you.
 
 
Transcription ended after 01:03:07

This editable transcript was computer generated and might contain errors. People can also change the text after it was created.
