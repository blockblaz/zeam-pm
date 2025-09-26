# Zeam Weekly call - Meeting Notes and Transcript
# Date: September 26, 2025

## Meeting Overview
**Date:** September 26, 2025
**Recording:** https://youtu.be/CTH8xCVDC0I

---
## Meeting Notes

Sep 26, 2025
Zeam call  - Transcript
00:00:00
 
Gajinder Singh: Hello everyone, welcome to September 26 Zoom call. we have made tremendous progress with Devnet zero and we will basically review what are the pending task. Most likely there are no pending task. We should be integrating spec test and we should be ready for for the interop. But again we'll go through and see if there is anything that is pending and apart from that we'll go for other updates and then we we can also stop talking about DevNet one. so to start off with we will go with updates starting from Kai.
Kai Chen: last week I mostly focused on the network spec change. the first is changed the topic stream to follow the latest lean spec and and I'm working on the snap task it will do the compress and decompress for current message and I'm also working on the to try to integrate the spec test but I still have some problem.
 
 
00:01:59
 
Kai Chen: So I want maybe so I want to have a chat with you later or sometime.
Gajinder Singh: What what kind of problem is it? Oh,
Kai Chen: two I think there are two kind two kinds of problems. the first one is about the the sample the sample the hard code config and there are some values not not exact exactly match zim code. I think this is one problem and another is some another is the in the zim code we we currently has have no validate attestation functions which in the spec test but I validate a test station.
Gajinder Singh: validate. Validate gossip
Kai Chen: I saw there are some similar code in the in the state transaction module, but in the spike test the validator attestation functions in the fork choice module they have some tests.
Gajinder Singh: Okay. So validate a test station we I mean so can you put up a spec test which is failing then we can look at it and see why it's failing because that is the more easier way to go and
 
 
00:03:53
 
Kai Chen: Yeah.
Gajinder Singh: see what is missing.
Kai Chen: Yeah. Yeah.
Gajinder Singh: What what configuration is not? Which values we don't have same as the spec test?
Kai Chen: Yeah. Yeah. Yeah. I can I can do that.
Gajinder Singh: Okay. But which particular values we don't have same as spec test?
Kai Chen: such as header and safe t target. it's it's not exactly miss match the spec test.
Gajinder Singh: All right. So just put up a spec test and so we'll basically look into it and see why we are failing. It's either our issue or it could be the spec test issue as well.
Kai Chen: Yeah.
Gajinder Singh: So just put up the test case and then we'll look into it.
Kai Chen: Yeah. Yeah.
Gajinder Singh: So I assume you will be able to put the test case soon enough.
Kai Chen: I think we we can I can first put put that
Gajinder Singh: because I would like to debug it today.
 
 
00:05:26
 
Kai Chen: I'm not sure it can is it easy to to fix or not.
Gajinder Singh: Yeah, don't worry about fixing. Just put up the test case.
Kai Chen: Yeah.
Gajinder Singh: All right, cool. I look into it and All right, so we we can now move to Guillaume.
Guillaume Ballet: yeah, so unfortunately I've been quite distracted this week. So no major update. I did merge Mercy's PR. I think that was this week. I been testing I've been in a battle to to try to get more people access to the to the metric machine. and it's a bit more difficult than I hoped. but yeah I hope to to have that running for for next week during the the interrupt. but yeah the metrics the metrics are working. I did demonstrate this last time. and otherwise I am currently going over all the PRs. I yeah I there's I started with one I had been open for for a long time.
 
 
00:06:48
 
Guillaume Ballet: It's not very relevant to the to the to the current effort of PQ testnet, but it's been open. So, I just want to merge it. it triggers a few problems, so I'm trying to fix them. Otherwise, I looked at the panic command with the cle help flag. So, I'm about to make a review for this. and I'm going to go over every PR and try to see what can be merged right now and what needs to to to have more input.
Gajinder Singh: Awesome.
Guillaume Ballet: Um, yeah, that's pretty much
Gajinder Singh: okay, I can go with my update. so I basically reviewed the PRs and I also thought that of a change that we can do in DevNet one. so I originally thought that it it might work for DevNet zero which is basically taking the signatures out and putting it in the in one field something that I talked about in the witness day call as well but then I realized that it might require some more change and thinking because one thing that I need to confirm is for the signatures that you don't get online and you get through block so so the signature that you will have for them for the votes that you will get through the block and not online.
 
 
00:08:07
 
Gajinder Singh: So the signature that you will get for them will be the block signature. Now the question is if if there is a vote that you are for example going to bundle in a block proposal then would the same block signature be used to signify that that a validator signed a particular valid vote in a particular block. Most likely I think it should be able to because if you can validate whether a vote was valid in the particular block using block signature then it basically you one should be able to use that same signature because we won't have the original signature now we would have the aggregated signature even though in DevNet one the signature aggregation is just simple it's just concatenation of the list. So there the list can be de-concatenated to get the vote signature. But if we do an actual aggregation then would that point the aggregator signature can be used along with the vote in the block because that won't have any signature. So that if you want to bundle that vote for example in a different branch or you want that vote to re-bundle because it helps to further finalization then basically you know would that signature work or not.
 
 
00:09:35
 
Gajinder Singh: So I just need to confirm that and once that is confirmed I will basically you know we can push ahead on this line of thinking. so I basically but so if since there is a little bit more thinking required on this I've made the target of signature aggregation for blocks to Devnet one so in Devnet zero which there are no more changes in the spec and then I'll look into why why the test cases for spec are breaking one of the things I think in the test cases that we should put up is the SSZ test. Kai, uh, do we have SSZ test in in the spec that you are testing Kai? In the spec that you tested, do we have SSZ test?
Kai Chen: Uh, no, no.
Gajinder Singh: Uh, no.
Kai Chen: I'm I'm working on the a test a testation a focus testation test.
Gajinder Singh: So, so can we first focus on state and block SSZ test along with the state transition because the fork choice can be a little off and it can still work out but the state transition needs to be totally in line.
 
 
00:11:11
 
Gajinder Singh: So I would basically want to see put up this test and do another PR where you put up the SSZ test for block and state and then another PR in which you do a state transition test. So I think we need SSZ test transition test and then we can basically move to fork choice test because going in that order will make sure that you know our step we are we are checking step by step right so if there is problem in SSZ or instate transition then it would cascade to definitely to fork choice. So instead of debugging for choice, I just want to make sure that you know we go step by step and make sure that other components are correct.
Kai Chen: Um, yeah. Yeah, it's it's okay. I I I just I I'm I'm working on this because the in in the spike dev TG group Toma raised this test. So I I'm Yeah.
Gajinder Singh: Right. Right. Yeah. But just let's put a basic SSZ block state test as well as state transition because I think that is sort of a unit test case of the spec test that we need to first figure out before we go to the fork choice test because fork choice is more like integration of various things.
 
 
00:12:48
 
Kai Chen: Okay. Okay. Makes sense.
Gajinder Singh: All right. But I I hope you can you know bundle these tests today itself because I want to take a look and debug them if there is an issue today itself.
Kai Chen: Mm- Okay.
Gajinder Singh: All right cool. okay then let's go to Noopur.
Noopur Singh: I have been working on moving the gossip messages from validator. So calling the publish from validator to node and I'm almost done like I'll publish sorry I will u I'll commit it like push it right now later like after the call that's it I just I
Gajinder Singh: Got And
Noopur Singh: just wanted to ask one thing so like while testing I noticed like u like some memory freeing is not happening properly. The there are some messages that I'm getting in and I was wondering if there is a if there is a bug logged or someone is working on it.
Gajinder Singh: yeah, at many places we have not freed memory just because we wanted things to patch up things.
 
 
00:14:11
 
Noopur Singh: so basically no it's like not being freed properly.
Gajinder Singh: And uh
Noopur Singh: it's happening that like null terminated slice is so there's a difference of one byte coming.
Kai Chen: Yeah.
Noopur Singh: So okay all right just wanted to know if like if it's on the radar.
Kai Chen: Yeah. I create I create one PR this Yeah. Yeah.
Gajinder Singh: Okay. Can you share me that PR? Because if there is a bug then we we would want to look at it and merge it in.
Kai Chen: Yeah. Yeah. I think it's a bug. It's a bug in the in the debug in the debug model. Yeah. Because I Yeah. Yeah.
Guillaume Ballet: yeah, you can you can assign it to me if you need want me to look at.
Kai Chen: Cool.
Gajinder Singh: All right. yeah, we need not worry when we have Guillaume. So let's move ahead. let's go to Chetany.
 
 
00:15:24
 
Guillaume Ballet: Maybe you
Chetany Bhardwaj: so this week primarily the work I did the major one was moving from start based allocation for SSZ to a heap based one. So basically moving to array lists from bounded arrays and that PR is on the SSZ side it is still pending review. So DM if you could have a look at it sometime. but on the Zoom side that has been integrated the current SSZ is pointing to the latest commit of this PR itself things seem to be working fine. I did some more testing in this PR as well and a lot of things right now they they seem good to me. So but in in case there are any edge cases or something that I missed that I'm happy to look into it. Apart from that u I did some I'm working on some minor changes and I've put out the PR. So moving from different preset for networks using flags that that is another PR that is up some testing PR and right now I just picked this one up.
 
 
00:16:41
 
Chetany Bhardwaj: This should be short one to this one is to expose a get default type from SSZ. So this one should be split enough and I should have it up sometime today probably.
Anshal Shukla: Yes. So, I was mostly working on Rox DB support and I have been able to do that. So, initially like I started a internal repo to like try it out and tinker around with the bindings. So and after that I have created the PR and Kai and Merci and Gajinder have already reviewed it. I have resolved all the comments that you guys have put up. and I think like it needs like another review before we can proceed to merging to merge it. And apart from that I have raised another PR which is around publish wrapper function and I have like basically cleaned the code little bit and there's like a bug related to memory management.
 
 
00:18:09
 
Anshal Shukla: So I think like I have resolved it but I had some concern concerns about like if we'll be able to test it or not. So maybe like another review on that will work. But yeah, that's about it. What I have done during the week.
Gajinder Singh: Thanks. We'll review your PRs and yeah GM if you can take a look at Prox DB1 for sure.
Guillaume Ballet: Yep.
Gajinder Singh: Cool. Let's move to Mercy.
Mercy Boma: Good afternoon. Am I audible? Am I audible?
Gajinder Singh: Yes, you are audible.
Mercy Boma: Okay. So, last the previous issue I was working on got merged. Then the my PR on SSC event, I had some issues because my I was only getting two 20 Hz from the test window I was running. But when I change the interval per slot to two, like I changed it from two intervals instead of four, it kind of passed. I sent you the log the I sent you the log on the chat.
 
 
00:19:24
 
Mercy Boma: I don't know if you can see.
Gajinder Singh: Yeah, you changed seconds locked. You mean you can't change interval plots per slot.
Mercy Boma: Yeah, I changed the interval spell slot in the constant zig file from 4 to 2. I increase.
Gajinder Singh: You will have to increase the time of your test because it needs to run to 23 for the first finalization to happen.
Mercy Boma: Yes, I increase the slots. I increase it significantly.
Gajinder Singh: Then in then remove your change in intervals per slot and push your PR if it's not finalizing then I'll need to have a look at it. don't change intervals per slot and update your PRs. Then I'll have a look at it if it's still failing. All right, let's move to Bing.
Bing T (bing): I I actually started looking into the hash zip crates and I was looking to create bindings for them because I was curious about about that part of the what do you call it the signature scheme basically. Yeah.
 
 
00:20:57
 
Bing T (bing): So I was actually working on that the hash sig crates.
Gajinder Singh: What exactly? Okay. Okay. Okay.
Bing T (bing): Yeah.
Gajinder Singh: Cool. Yeah. See 23 slots are like 4 into 23 seconds which is like 3 minutes maybe.
Mercy Boma: Sorry, I wanted to ask because I changed it to 10 minutes that's 600 seconds. Should I increase it further? I should add extra 3 minutes to it to the 10 minutes.
Gajinder Singh: Yeah, just push your beer if I'll take a look at it. Okay, cool. Uh, let's move to Hakeem. Hakeem, you are in this call for the first time or Okay, cool.
Hakeem OREWOLE: No, I was here last week. Yeah. Um, I've been like working on little tasks that were assigned to me. Um, I would like to know how task are assigned because I maybe over the weekend I would prefer to like work on a more maybe a test or bug fix or a feature a small feature just to get more familiar with the code base.
 
 
00:22:36
 
Hakeem OREWOLE: I've actually also been playing around with the code and yeah I realized some weird cuz regarding memory the memory issue that was discussed earlier I've actually been trying to comment out some deallocation functions and they seem not to affect the code. I don't know if I can speak to someone to explain what is actually going on. Then I would like to ask some questions regarding is there room for pair programming?
Gajinder Singh: All right.
Hakeem OREWOLE: Is anyone on the call doing pair programming?
Gajinder Singh: I'm not sure who is here for prayer programming but yeah it's welcome if anyone wants to do pair programming for sure I mean the way the payer programming automatically happens is through reviews in some sort of a sense but somebody has to run the point on something right so this is where you say that I'm doing this task and then if you have issues then people jump in to solve that right so then that
Hakeem OREWOLE: Okay.
Gajinder Singh: is only the pair programming happens and how task are allocated.
 
 
00:23:51
 
Gajinder Singh: Basically you choose what you feel like you will be able to do and what interests you on the basis of that and basically just message in the group unless I or someone else assign you the task and say that okay you know we need this on priority can you handle this which is again a which is when you can say that okay I want to do this or not or you'll be able to do this or not so as such it's pretty open the way the tasks are handled just pick something and message in the group that you want to handle this and then probably someone will say that okay you can handle this or you can't handle this because you need this to do this or you know people will jump in with their knowledge and help you out over there.
Hakeem OREWOLE: Okay. All right. Thanks. That's That's fine.
Gajinder Singh: Cool. Yeah. And thanks for your contributions. Yeah, looking forward to more of them.
Hakeem OREWOLE: Yeah, hopefully I'll be able to contribute more.
 
 
00:25:01
 
Gajinder Singh: Okay, I guess we can now go to Ladislaus
Ladislaus: Yeah, I don't I don't have a major major update for for this group. Maybe just in light of the of the upcoming workshop sort of create a little bit of excitement and sort of a zoomed out goal for for this group is like looking at the agenda that that that Justin has put together. It's it's a packed workshop. It's we have sessions on on the DevNet specs on on peer-to-peer performance. We have we have a key gen key key generation session. We have a lean peer-to-peer session, a lean metric session, a binding session, multiple like DevNet breakouts. and I guess we we're closing things off with a with a demo hopefully of of DevNet one. So this is where sort of all you guys' contributions here in this call fit in. We also like are working on recording every single session. so fingers crossed that this is going to work out to to to to share it. obviously and yeah I guess what what I'm particularly keen on on sort of working on is sort of integrating not just like Ream and and Zeam sort of the regulars like you guys who've been pushing lean consensus forward but we have a like a few other client team devs like from the clean team for example we have someone from from prism will be there so it's just hopefully like
 
 
00:26:41
 
Ladislaus: by by sort of yeah, taking this workshop as a bit of a shelling point to yeah to to get to get even more more developer and more more mind shares mind share and and sort of more developer power behind this. and yeah going out of the workshop obviously having having ideally sort of solidified our our our vision for for not just DevNet one but but but also the follow-up DevNet. So yeah, I guess this is all this is to say that there's like it's a very cool moment in time I would say to to build on le consensus and sort of we hope that this week will sort of really really move the needle forwards on on a much larger scale. You
Guillaume Ballet: yeah, so I'm sorry to ask for for this, but apparently I have sunk to a level of stupidity that I'm not able to take a simple plane ticket. I I think there's a bug in their system. and the the one flight I'm trying to get does not exist because I try to book it through five different platforms and they all reject me.
 
 
00:27:49
 
Guillaume Ballet: my question is, you know, [REDACTED]
Ladislaus: I think for you [REDACTED], that would be really good.
Guillaume Ballet: Okay, that's my I was hoping to make it before that, but yes, that's Okay, thank you for the deadline update.
Ladislaus: Sure.
Gajinder Singh: Awesome.
Ladislaus: Yes.
Gajinder Singh: And and from one thing that Ladislaus has brought up is that other client teams joining this initiative and I have so we already know that Lighthouse has joined it and I think Brandain has also becoming active.
 
 
00:29:03
 
Gajinder Singh: So yes more the marrier. and the second thing that Ladislaus brought up was about metrics and something that is we need to push on it. Now we have the metrics note up by by the re guys. Maybe I think we should take a look at it and see if it all makes sense or if there are any missing metrics as well as start implementing them. I don't see think that it would be it would be a challenge to implement any of them but it still needs to be u to be completed. So maybe Mercy can take a lead on this after we finish up with the integration PR and maybe Hakeem can also sort of join in. Yeah. Lettuce, you wanted you were saying something.
Ladislaus: I was that's that was sort of my I guess very high level motivation update for for this group.
Gajinder Singh: Yep. It's quite amazing and from where we have started and to where we have reached.
 
 
00:30:23
 
Gajinder Singh: Yeah. Merci.
Mercy Boma: So I wanted to add I did check the I did check the lane the Ream metrics. I wanted to know if we have things we want to see on our own end that we can include like what we currently have is what you're suggesting. Do we have any other thing we want to see on our own end cuz I did go through the the hackmd file.
Gajinder Singh: Yeah. Do you have I haven't checked it. I haven't reviewed it, but do you have any suggestions that you think are missing?
Mercy Boma: Yes, I have. Yes, I I'll put that in the I will put that and send to you.
Gajinder Singh: All right. Right. Right. Cool. also you can I think start comments on that particular hackmd itself. So but yes do let me know as well
Guillaume Ballet: I mean, I would not rush to I mean, of course, if you have ideas, please add them. I wouldn't rush to fill them because I'm pretty sure like running this whole thing as an interop is going to cause some bugs and this is when we're going to say, "Oh, this is what we need. This is what you know, there there's no need to try to second guess everything.
 
 
00:31:36
 
Guillaume Ballet: like I said, if you do have ideas, please go ahead." that's great. but yeah, don't don't stress over that. We will find plenty of metrics to to measure when we when we run into problems.
Gajinder Singh: Right. Right. And I mean as Guillaume said that there's no need to second guess. There are some things that are very obvious and we should have them. so I mean we just scrunch them out. and as Gim said and when we discover more of the things then we will also figure out what other things that we need to monitor. With regard to that we have one PR from Scotty for viewing the fork choice tree and there are some comments that I added on it and if someone wants to complete it because if we have the fork choice tree visualization in the logs itself it is it becomes very easy to debug things. So if someone is interested to sort of complete the PR because Scotty did it and didn't follow up.
 
 
00:32:44
 
Gajinder Singh: So I guess that would be good or if someone wants to take it a bit further and say you know you can integrate fork choice you can integrate some grafana you know graph that can shows the fork choice visualized over there that is also very that would also be very nice so I haven't seen any of this stuff in grafana But if you guys can check it out and if not then maybe some other visualization tool that can you know generate the entire tree along with the weights. I think that would be very cool and very helpful in debugging something that you can spin up with electron kind of a browser tool. I'm just guessing. Okay. all right guys, I guess we had a very productive session today. So let's the focus is on spec test a little bit on on the metrics because we do want metrics and yes to basically also make sure that most of the things we can run on CI and sort of to bring other things together that we can start prepping for DevNet one because for the workshop we would be at point where we can say that okay you know we can start doing things at devnet one.
 
 
00:34:29
 
Gajinder Singh: So that would be the focus and so basically the focus of ne next week is to complete all those things and make sure we are able to do the interrupt. I will check with guys if their client is ready for interrupt and if there that is then I'll try to interrupt with it. one of the other things that is in my mind and that has not been in Zen client is syncing the parent chain. That is something that I wanted to add that if other things if not much of my time would be gone in PRs then I would try to focus on that as well. So that will be all and yep see you guys next time I think. the badness day post and drop calls are not happening for next two weeks. but we will have yeah but we will have the zoom call next week as well. So we can have the Zoom call next week. Uh, all right.
Gajinder Singh: And by the way now our Zeam calls are published on YouTube and our YouTube channel is ZeamETH I think. so cool.
Noopur Singh: Yes, I'll share the YouTube channel on the group as well.
Gajinder Singh: All right. guys, thanks for being here and see you next time. Yes, Mercy. We'll go get to that as well. Thank you.
 
 
Transcription ended after 00:36:51

This editable transcript was computer generated and might contain errors. People can also change the text after it was created.
