# Zeam Weekly call - Meeting Notes and Transcript
# Date: May 08, 2026

## Meeting Overview
**Date:** May 08, 2026
**Recording:**

---
## Meeting Notes

May 8, 2026
Zeam call - Transcript

00:00:00

Gajinder Singh: Hello everyone, welcome to May 8 Zinc call. Uh let's look into our DevNet 4 uh performance regard to aggregators and we will then look into a little bit about our DevNet 5 preparation. Uh so we will intermix it with the updates and talk about it as we go along. So let's start with I'm jealous.

Anshal Shukla: Yeah. So, uh after like last week I completed my ZEG upgrade PR uh that one got merged. uh alongside that like I was uh working on the sidelines for DevNet 5 uh regarding like API finalization and regarding like the consensus mechanism uh understanding goldfish and other uh documents and discussions that we had within it. Uh and on uh apart from that like I was also involved in reviewing a few PRs that Partha raised for Zig FFI. Uh there was another contributor who has raised one PR uh regarding uh consuming config file via JSON via via uh command line argument. So yeah got that merged and a few ones here and there. uh right now like I'm working on I'll like start working on uh the spec side of things uh for multi mass uh multi message aggregation uh and uh I'm also like continuing to look on to like the consensus side of things there are like a few things on Z side that I have done partially but don't have like a solid update on is one of them is that I need to I still need to work on removing the schedule on root loop functionality completely because that was

00:02:00

Anshal Shukla: just that is just being used uh by beam command and we can uh simulate like think of a alternate simulation mechanism and avoid lipex there uh because it's like uh making the current code base complicated and I think like the testing part should be completely segregated. uh apart from that like we have thought about like various three factors that we want to do do. So I was also looking into the threading architecture of uh lighthouse but yeah these are some things that like still not fully done. So uh don't have like a conclusive update on them. uh apart from that like I'll just remind you Gajinder that we have the PR regarding uh regarding uh caching of Merkel roots on SSD side. So maybe once I can get a review on that I can uh resolve it and update you and the same side as well.

Gajinder Singh: Got it. So I'll look into that SSDBR and can you also ping Yum on the channel to look into that because he will

Anshal Shukla: Yeah, I'll I'll ping him personally.

00:03:13

Gajinder Singh: definitely

Anshal Shukla: I had P earlier on the channel,

Parthasarathy Ramanujam: All

Gajinder Singh: Right.

Anshal Shukla: but didn't receive a response.

Gajinder Singh: Right. So just ask him to relook at it.

Parthasarathy Ramanujam: right.

Gajinder Singh: And uh with regard to DevNet 5 uh where are we have we basically got all the changes that we needed to done from

Anshal Shukla: So we have gotten like the API side of things done.

Gajinder Singh: Emil

Anshal Shukla: uh he has those changes on the main uh branch uh until now but he's working on integrating uh like creating a separate DNET uh five branch on uh lean multisig itself which will basically uh enable it with lean sig uh repository. So they have they have continued to maintain that main separate main and definite specific branches and the signature scheme there is different. So he'll be doing that and uh along alongside that I still need to have like uh the testing test config leanig integrated as well so that I can uh update my bindings and have it uh have it in lean swag but since like the APIs are almost uh final like APIs are finalized so I can still continue with the dummy data and uh proceed with the uh spec changes as such.

00:04:37

Gajinder Singh: So the changes that we wanted in the split API are they there or are they still to be

Anshal Shukla: Yeah,

Gajinder Singh: done?

Anshal Shukla: They're

Gajinder Singh: Okay. All right.

Anshal Shukla: done.

Gajinder Singh: Uh so let's do this and let's do the spec for this and uh in the meantime let's yes look into goldfish and how we can integrate it and then propose maybe definite 6 with that or a separate PR on it. So, we'll see.

Parthasarathy Ramanujam: Let's

Gajinder Singh: All right, let's move to Kai.

Kai Chen: Uh not uh not much uh date this week. Uh not feel not fair well this week. So uh just uh just ask ask uh zero uh add the blocks by range uh RPC and uh and um uh make some suggestion but uh for the uh S thread model refactor. uh in the uh chub uh

Gajinder Singh: Awesome.

Kai Chen: comments. Yeah. So I think um yeah I think right now uh PASA is working on that. I think uh the the new design is uh is more better than than before.

00:06:25

Gajinder Singh: All right. And uh what are you focusing on as of

Kai Chen: Um,

Gajinder Singh: now?

Kai Chen: no. Uh, I'm just uh look look at the proposal by for five.

Gajinder Singh: Right. One other thing I wanted to ask Kai was is Ziggly P2P ready or when do you think it will be

Kai Chen: Uh current currently

Gajinder Singh: ready

Kai Chen: no because uh we don't we don't have uh much resource uh working on that uh and

Gajinder Singh: status for Zig clip it

Kai Chen: Um so we have we

Gajinder Singh: away.

Kai Chen: have uh we uh a beacon client um code by uh open claw in in no at at Nordstuck and it used used the DPP but uh but found several issues with uh interop with uh lighthouse. Uh so the may the may repo the uh so maybe not not works right now but that that fork uh cannot be used also.

Gajinder Singh: Got it. All right. Okay. Let's move to

Parthasarathy Ramanujam: Uh yeah.

Gajinder Singh: Pa

00:08:36

Parthasarathy Ramanujam: So last week uh spent most of the time testing multiple subnet uh uh DevNet identified quite a few issues on Ze and other clients and reported them. uh worked with Zclaw uh Anel and Kai on the mutx uh related changes uh which has now gone live and uh Ze is a lot more stable than what it used to be. There are still a couple of minor issues that we have to address which hopefully will be done after we merge 848 and 850. uh one of the problem presents itself only when Ze functions as an aggregator because uh it appears to reject certain blocks which other clients seem to accept uh on the analysis. I think it's because uh the underlying uh parent route of those blocks are not available within Ze when it functions as an aggregator which results in a uh I mean a forking of the chain and then

Gajinder Singh: So all this all this should be a visible in the log because we have log for this when we reject block

Parthasarathy Ramanujam: uh

Gajinder Singh: because we

Parthasarathy Ramanujam: yeah the logs are there that's how I created the issue and then ZCLO created the appropriate

00:09:43

Gajinder Singh: don't

Parthasarathy Ramanujam: uh fix as well but I'm not sure why the parent route is not available uh or I mean because I I'm not yet conf uh confident with the uh consensus logic.

Gajinder Singh: Excellent.

Parthasarathy Ramanujam: I'm not sure if the fix that was applied by Zclaw is correct. So I

Gajinder Singh: So you we log out our forest tree.

Parthasarathy Ramanujam: won't

Gajinder Singh: So you can actually see whether in the last log out of for tree you have seen

Parthasarathy Ramanujam: yeah it's seen there.

Gajinder Singh: that

Parthasarathy Ramanujam: It's there in the log but the fix that Zclaw applied I'm not sure whether that is correct. So, I want either of you to have a look and confirm. Uh 8:48

Gajinder Singh: I mean then you say that the parent root is not there.

Parthasarathy Ramanujam: as

Gajinder Singh: There is just a simple way to confirm. You can look at the uh the the forchoice log that we do in the So do we do you see that particular route

Parthasarathy Ramanujam: Yeah.

Gajinder Singh: in there or

Parthasarathy Ramanujam: No. No. It the uh it's correctly rejecting uh that the root is not available.

00:10:42

Gajinder Singh: not?

Parthasarathy Ramanujam: But my question is why is it that only Ze seems to report that it's not available when it's when the same slot is acceptable accepted by all of the other clients. So that leads me to believe that somewhere in Zen's logic we seem to have not ced up an earlier parent correctly or or we have a different vision of

Gajinder Singh: No,

Parthasarathy Ramanujam: the

Gajinder Singh: I mean I mean that that means that so so you if the parent root can you check in the log itself that we ever received that parent route or not and what happened to that log?

Parthasarathy Ramanujam: No, we haven't. Uh I that's something I have to check. I I I haven't checked that yet. But uh when the message came out, it's accurate that Ze did not have that log. So it rejected it. Um so and this happens only when Zam functions as an aggregator. Zam 1 was a regular validator. It uh accepted that particular uh route and then was able to proceed. But the aggregator did not which is possible because the aggregation um slot interval did not trigger on time.

00:11:53

Parthasarathy Ramanujam: So it failed to accept these incoming uh slots from the peers and that that's that might have been a a reason for it to miss but I don't

Gajinder Singh: But we also uh cached them,

Parthasarathy Ramanujam: know

Gajinder Singh: right? The feature slot block, we also cach it.

Parthasarathy Ramanujam: the

Gajinder Singh: So it should be replayed afterwards.

Kai Chen: Uh, is is it is it because the PR 754 not merge because the SP some

Parthasarathy Ramanujam: Uh oh.

Kai Chen: change?

Parthasarathy Ramanujam: Okay. Uh I that might be one reason. I'll I'll take a look at 754 and see if we can merge that as well. Are you happy with the changes there? Uh Kai

Kai Chen: uh because they uh it include two maybe it include two changes but I think it's okay so maybe I maybe lead

Parthasarathy Ramanujam: Yeah.

Kai Chen: agenda to confirange

Parthasarathy Ramanujam: Okay. There are some conflicts on

Gajinder Singh: So in in PR in PR 754 why are we removing pending

Parthasarathy Ramanujam: that.

Gajinder Singh: blocks?

Kai Chen: Um, so because because the because I check recheck the the latest spike and the other client code that the so

00:13:28

Gajinder Singh: So this the this pending blocks was for our uh

Kai Chen: Yeah.

Gajinder Singh: slot not our clock being not not triging at the right time. Right.

Kai Chen: Yeah.

Gajinder Singh: So I I I mean this we don't accept the feature blocks. We put it in the pending blocks only because our local choice clock has not ticked

Anshal Shukla: So I think what Swatham just mentioned can be because of the aggregation part

Gajinder Singh: ahead.

Anshal Shukla: taking uh quite some time and uh validator uh aggregator not being able to uh fulfill its duty that had to be ticked by other clocks. So it it does it sync it aggregates it synchronously and it takes more than one interval to actually complete the aggregation. And this is like related to the other blogs that uh we were discussing that lambda classes uh published. So uh they they do the aggregation part asynchronously and later on like they have a time limit on that before they stop aggregating more uh more payloads. And uh if when we try to do this synchronously and it doesn't complete within 1 second it basically just holds on to the clock and doesn't let it tick because it's like uh it's a blocking call.

00:14:53

Anshal Shukla: So uh this is something that uh that was another thing that I forgot to mention earlier but I have to uh fix but I was hoping that I was hoping to look into like the new async uh paradigm that Zigg has reintroduced so if he can utilize it but again I couldn't do it. Maybe I'll just ask Zclass to handle it for us and review

Gajinder Singh: So do we have metrics for our aggregating time?

Anshal Shukla: it.

Gajinder Singh: What is the metric for our aggregation?

Parthasarathy Ramanujam: Yeah.

Gajinder Singh: What is that?

Parthasarathy Ramanujam: Uh mostly it was it fires within8 but in some occasions it can proceed up to 1.2 seconds.

Gajinder Singh: No, the aggregation time for a time to aggregate

Parthasarathy Ramanujam: I I'm referring to the interval. uh the actual aggregation maybe is not

Gajinder Singh: Not what is the time to for actual

Parthasarathy Ramanujam: uh uh I don't think we've

Gajinder Singh: aggregation?

Parthasarathy Ramanujam: measured that un did you have a chance to look at it from our glue how much it

Gajinder Singh: It is very likely there is a matrix for

00:15:53

Parthasarathy Ramanujam: takes

Anshal Shukla: I think there's a metric for it but I'll have to look into it.

Gajinder Singh: it.

Anshal Shukla: I didn't look into it. I just knew I got to know about this issue in path mentioned right now.

Gajinder Singh: Yeah. So the PR that Kai mentioned anyway will not solve the issue because it is further trying to reject the block, right? We are accepting those blocks to pending blocks but uh the that your problem is

Kai Chen: Yeah.

Gajinder Singh: different right and it could be very well that we were not able to report that block in the time in time and uh yes so from the logs we should basically also confirm it that we were not able to import those uh that block or was uh some thread stalled doing the compute in the sense that li P2P did not deliver us messages or whatever. I mean I don't know this needs to be figured out that why are we not getting it and obviously aggregation needs to happen in time and uh we also need to run a timer on the aggregation uh and exit it once the timer runs out.

00:17:07

Gajinder Singh: Right? So if for example it's taking 3 seconds to aggregate obviously aggregator is not working but we

Parthasarathy Ramanujam: Okay.

Gajinder Singh: also know that that aggregator after that after the interval is passed it's not going to do any useful work so that aggregation is no more useful

Parthasarathy Ramanujam: No.

Gajinder Singh: anyway or

Parthasarathy Ramanujam: So these two PS

Gajinder Singh: is it use it could be useful as well. So, so doing it on a separate thread might make sense like the way Inel has mentioned that

Parthasarathy Ramanujam: Yeah.

Gajinder Singh: uh uh when the interfs you basically trigger the aggregation and that happens on a separate thread and when that completes it will basically automatically post the aggregates to lip right. So,

Parthasarathy Ramanujam: Yeah.

Gajinder Singh: so this basically this uh so we definitely need to separate out our clock running thread

Parthasarathy Ramanujam: Aggregation.

Gajinder Singh: from the compute threads right

Parthasarathy Ramanujam: Yeah.

Gajinder Singh: so as of now it seems like aggregation is what is an issue so let's span a different thread for it when we trigger the interval for aggregation and uh basically depending upon whether the result is a still within slot or not we

00:18:26

Parthasarathy Ramanujam: Yeah.

Gajinder Singh: can post the aggregate or not and I think we can generally post the aggregate because even if your aggregate is a bit delayed at least others will get votes delayed votes

Parthasarathy Ramanujam: Yeah. So I can do that as a follow-up PR once uh this 848 and 850 lands. Uh so if you can uh approve this I mean just uh have a look at it I can proceed after that.

Gajinder Singh: I was looking at one of the so uh one of the things was that you just

Parthasarathy Ramanujam: I'll do the test.

Gajinder Singh: moved so it just moves clean up to some of the function but I haven't looked below that I don't know what else it's doing right just moves the uh finality advancement to some

Parthasarathy Ramanujam: Okay.

Gajinder Singh: other function which is called run periodic pruning which does the same thing but I don't see what has happened below that I haven't seen

Parthasarathy Ramanujam: Are you referring to 848 or 850? Uh,

Gajinder Singh: 848 I mean

Parthasarathy Ramanujam: okay. I mean, we can take this offline.

00:19:42

Gajinder Singh: Has it changed?

Parthasarathy Ramanujam: I'm just No,

Gajinder Singh: Has it changed anything in the pruning itself or it has just moved

Parthasarathy Ramanujam: it just reorders the way in which uh it's done to ensure that uh couple of them are metrics

Gajinder Singh: it?

Parthasarathy Ramanujam: related changes to capture something. The other is order reordering the way the pruning happens and uh uh the actual uh what do you say the change related to

Gajinder Singh: So what does it re on

Parthasarathy Ramanujam: Yeah.

Gajinder Singh: All right, coming to my update. Uh, I've been looking at PRs as well as uh looking at DevNet five spec discussions with Anchel and also looking into uh goldfish uh with Jan as well as Anel. And so that has been my update. All right. Uh I think Napur do you have anything to chime

Noopur Singh: Um,

Gajinder Singh: in?

Noopur Singh: no I was just busy with open claw and configuring something and experimenting with things.

Gajinder Singh: Great.

Noopur Singh: I do not have anything.

Gajinder Singh: Right. So one thing uh to mention is that uh since we have shifted to a higher version of the model, we have we consuming a lot more tokens now and it is getting expensive.

00:21:04

Gajinder Singh: So we might need to figure out how to make it less expensive. I saw something which was about compressing the tokens as well. So maybe that could help us uh a scale that helps compress the tokens and trying other uh u other engines, right? Other providers. So we'll see. We'll see what where we land on that. Yeah, get caveman. Right. So, we'll see where we land on that. And uh I think the best thing would be to figure out a locally run model which basically learns our codebase over time as well. But uh I don't know how what is the state of art on that. GM was doing something similar earlier for something else and uh we'll basically sync up with him that what he has been able to achieve. All right.

Noopur Singh: Okay.

Parthasarathy Ramanujam: you need a local server running GPU and then install local AI I think which

Gajinder Singh: Uh

Parthasarathy Ramanujam: lets you install Opus model but uh again it might cost probably $200 per month uh for the server.

00:22:30

Gajinder Singh: So I do have local server. I was able to procure it and I mean we we have our

Parthasarathy Ramanujam: Okay.

Gajinder Singh: uh zcloud running in a server right. So uh which is again our server right.

Parthasarathy Ramanujam: Okay.

Gajinder Singh: So we I have a local server. I have GPUs if it needs GPUs.

Parthasarathy Ramanujam: I I think there's called something called local AI which uh you can run which allows you to uh

Gajinder Singh: Uh

Parthasarathy Ramanujam: install Opus uh LLM and then import the tokens. But this is purely an experiment that I heard somebody was building on their

Gajinder Singh: all right. So no,

Parthasarathy Ramanujam: own.

Gajinder Singh: can you just take a look at that and

Noopur Singh: Yes. Okay.

Gajinder Singh: see

Noopur Singh: And anyway like meta also provides some uh which you can run but I don't trust meta.

Parthasarathy Ramanujam: Yeah. Llama, right?

Noopur Singh: Yeah. Llama. Yeah.

Parthasarathy Ramanujam: Llama. Yeah. Oh,

Noopur Singh: So,

Parthasarathy Ramanujam: Llama and Lama.

00:23:24

Gajinder Singh: Right. Right.

Noopur Singh: okay.

Gajinder Singh: I think uh eventually we'll have to go towards that kind of model because the cost for AI tokens is going to be prohibitive as of now and maybe it will become more costlier as things go along.

Parthasarathy Ramanujam: Yeah.

Gajinder Singh: So yes and also I think it would be better for something to know Z as a project right and not to start from a blank state

Parthasarathy Ramanujam: Yeah.

Gajinder Singh: even if we have some memory or uh some instructions in our agents empty. Yeah. So so we'll see where we go from over there.

Parthasarathy Ramanujam: Yeah.

Gajinder Singh: But yes uh quite excited to see that we are releasing a lot of code with the eye and uh I guess we are a little ahead of curve from everyone else even though other people are also doing it but uh I'm assuming that we are doing it in a most productive manner. So thanks uh for today's call. Anything else we have to discuss? All right guys, I think uh we still pa need to further do a very rigorous subnet testing.

00:24:45

Gajinder Singh: I don't know when we'll do that.

Parthasarathy Ramanujam: Yeah.

Gajinder Singh: We just need to figure out first to make z computer as stable as possible and

Parthasarathy Ramanujam: So

Gajinder Singh: let's look at all these issues and figure them out.

Parthasarathy Ramanujam: yeah, I mean as soon as I have two clients that are very stable running as an aggregator, uh I can scale up uh to multiple subnets even up to 500 or thousand server uh

Gajinder Singh: So we we I mean we can just focus on zam right.

Parthasarathy Ramanujam: nodes. Yeah.

Gajinder Singh: So zam as sorry z multi-subnets right.

Parthasarathy Ramanujam: Yeah. So

Gajinder Singh: So we anyway have to do that as well multiple subnets using zam.

Parthasarathy Ramanujam: thank you.

Gajinder Singh: I think that should be priority rather than trying to figure out things for other clients.

Parthasarathy Ramanujam: Okay.

Gajinder Singh: uh they got their own resources to that seemingly and uh let's let's just focus on this and let's uh make sure that Zim is the most stable out there.

Parthasarathy Ramanujam: Sounds

Gajinder Singh: All right, guys.

Parthasarathy Ramanujam: good.

Gajinder Singh: And it's Yakai, you wanted to say something, Kai?

Kai Chen: Uh, no.

Gajinder Singh: All right. Okay, guys. Then see you next week on May 15, same time. Till then,

Noopur Singh: Thank you.

Parthasarathy Ramanujam: Cheers.

Noopur Singh: Bye.

Parthasarathy Ramanujam: Hi

Gajinder Singh: take care.

Kai Chen: Bye-bye. Thank you.

Transcription ended after 00:26:06

This editable transcript was computer generated and might contain errors. People can also change the text after it was created.
