# Zeam Weekly call - Meeting Notes and Transcript
# Date: May 15, 2026

## Meeting Overview
**Date:** May 15, 2026
**Recording:**

---
## Meeting Notes

May 15, 2026
Zeam call  - Transcript
00:00:00
 
Gajinder Singh: Hello everyone, welcome to May 15 Zoom call. Uh we will continue discussing uh debugging runs for DevNet 4 and DevNet 5 preparation uh as we go along taking updates from everyone. So let's start by Unshell on DevNet 5.
Anshal Shukla: Yeah. So uh so on devet 5 like I have a spec uh it is functionally ready but I am facing still facing some issues while running it on CI. So the issue is that the there's one test in which all the constru uh in which the block actual block building and uh and verification happens. So the fixture it is one of those fill fixture tests. It fail it doesn't fail but it takes too long to run on a uh run on the CI. Even locally it took me like 2 minutes but on but on CI it takes like 20 30 minutes to run and I had to like decrease the number of max stationation. So I brought it down from 16 to 8 which was anyways our target. So uh that improved a little.
 
 
00:01:26
 
Anshal Shukla: So earlier I it used to take like 15 minutes on my local machine. Now it has come down to like 7 8 minutes which is considerable improvement. But I think the bottleneck there is the RAM because uh the CI build uses quite less RAM plus the default configuration on which we run those fill fixture tests has a Linux machine and it's like significantly so slower on Linux machine as compared to Mac. I think the default configuration of uh Mac runners, GitHub action runners is uh gives us like 14 GB of RAM but the default of Linux G gives only 7 GB. So that is also something that is creating an issue. So uh locally I can see like I am able to aggregate even type two signatures within 2 3 seconds at max like it takes like 10 12 seconds but still it's taking quite some time and uh I think it's a memory issue because when I try to run it parallelly uh then it is in fact slower if I do like a single process run. So that's why I suspect that it's related to memory.
 
 
00:02:45
 
Anshal Shukla: Uh I plan like I have some ideas to improve it. One of them was like decreasing the maxist stations with decreased it decrease the time but it it still takes like 2 hours and it is yet to completed which is anyways not something that we would want to do on a spec side. Uh another thing that I can do is maybe I can set even lower maxist stations for test suit or I can like move it from Linux. Uh so right now we generate these fixtures on Linux uh GitHub runner. I can change it to Mac OS. That is one thing but I think like it is not I think it is good if we can move to like a more heavier machine on the CI side as well.
Gajinder Singh: So, how how do we move to a heavier machine on CI
Anshal Shukla: So GitHub does provide some plans. I haven't explored it but if we can do that then I can see how we can like there are some preferred plans using which we can move to like a heavier machine but uh I haven't gone through that.
 
 
00:03:59
 
Gajinder Singh: So, lean ethereum repo needs to get these plans, right? Yeah.
Anshal Shukla: Yeah.
Gajinder Singh: So, I guess you can suggest it uh to Toma and in the meantime I
Anshal Shukla: Okay.
Gajinder Singh: if lower maxration works that is fine as well, right?
Anshal Shukla: Yeah.
Gajinder Singh: for yeah for this CI configuration right so if we
Anshal Shukla: I Mhm.
Gajinder Singh: can have max stationations as part of config variable I think that would be better and if uh is this a part of config variable config variable config
Anshal Shukla: Yeah. Yeah, it is part of config variable. But I think some of the test cases might fail if I decrease it further
Gajinder Singh: variable
Anshal Shukla: down. But I I'll check it out locally because local runs are still significantly faster. So it won't take much time.
Gajinder Singh: So is it about not uh the runs are failing or just runs are taking more time?
Anshal Shukla: The runs are taking a lot of time.
Gajinder Singh: Okay, I guess uh with toma you can basically figure this out.
 
 
00:05:03
 
Gajinder Singh: But as such PR is ready. Okay, if the PR is ready uh Kai can we start the implementation for
Anshal Shukla: Yeah.
Gajinder Singh: that?
Kai Chen: Um yes.
Gajinder Singh: All right, cool. uh let's do demonet five implementation and yeah the mean dimensions you can resolve the issues that are coming around it in
Anshal Shukla: Yeah, I did like quite some benchmarking on the aggregation side because I thought like
Gajinder Singh: respect
Anshal Shukla: that is the culprit and it was the culprit if we increase the max statistications to 16. So if I try to fold like uh 16 attestations into one block. So merging that takes like quite some time about uh 2 to 3 minutes. So that's why I decreased it to eight because it is along along the target that we had initially aimed for and that is something that is already being used on the on the beacon chain as well. So I think that is fine. But if I decrease it further down then I think I'll just do it for the test.
Gajinder Singh: Got
 
 
00:06:28
 
Anshal Shukla: Yeah.
Gajinder Singh: it.
Anshal Shukla: But from that like I have raised a PR uh for uh to remove like scheduling of gossip network uh calls on the libex thread. So I think uh I think Pat has already reviewed it and he has put some minor comments there. So I'll address them. Uh also like I noticed that uh GM has also reviewed my caching PR. He has also suggested some changes. So I'll be I have like worked on some of them. Some of them are still pending. So yeah, I'll continue doing that.
Gajinder Singh: Okay. Can you part and k sort of colleate a performance related uh issue where all the todos you know we can put all the to over there and
Anshal Shukla: Sure.
Parthasarathy Ramanujam: Sure.
Gajinder Singh: crunch them one by one because I feel that the performance still needs to go up. Arthur, have we started running multi-threaded workers or
Parthasarathy Ramanujam: Yes, we have.
Gajinder Singh: not?
Anshal Shukla: Thanks.
Parthasarathy Ramanujam: So uh I mean I if Anel is done I can probably give my comment because they're probably
 
 
00:07:38
 
Gajinder Singh: Yeah.
Parthasarathy Ramanujam: related. So uh after I spoke to you Gajender I ran um uh what do you say
Gajinder Singh: Yeah.
Parthasarathy Ramanujam: uh four devet uh sorry uh with 64 128 machines and uh with each it's a homogeneous uh subnet uh with four clients. Um so uh from what I noticed uh I just wanted to understand uh and all of the observations and issues that were identified in this subnet uh uh run has been logged in issue number 863 and based on which I have uh also created few PRs which have been added and merged right now there's one outstanding one question I had for you Gajender is that when we had a discussion a few weeks ago you mentioned that uh um you wanted uh a configuration parameter where You could specify the list of subnets a particular uh client can aggregate to but it has to only uh do the attestation or rather listen to attestations from other subnets but it should only gossip to its own subnet right uh is there a reason behind uh this topology what was your reasoning behind it um the reason I'm asking you is that this is leading to a problem with the uh ze when it's functioning as an aggregator.
 
 
00:08:58
 
Parthasarathy Ramanujam: uh when I saw uh uh Zen uh client as an aggregator was listening to over 54 um clients or peering with 54 clients for uh aggregation which resulted in uh severe degradation of uh the process uh which uh I'm slowly trying to address but most of it were due to a long block on the gossip uh thread where it's not able to find uh the block and It requests each block as blocks by root which is failing. So I've also added in my latest PR um blocks by range if possible and few other uh minor validations where it skips uh at the very top instead of waiting and doing it later. Uh but a easy
Gajinder Singh: Why why these requests are not happening on a separate thread?
Parthasarathy Ramanujam: fix
Gajinder Singh: I mean why are they happening on the main thread? Why can't we whenever we are requesting it I should have an on a separate thread
Parthasarathy Ramanujam: So that that is the change that 886 uh uh I mean the PR that I had uh
Gajinder Singh: right
Parthasarathy Ramanujam: written yesterday is uh doing it delegates part of the work to a separate thread and the main thread is used only for I mean or rather the lib XCV thread is the main one.
 
 
00:10:14
 
Parthasarathy Ramanujam: Um so uh some improvements are there being done there but uh I wanted to understand the topology why when uh it is sufficient for uh an aggregation to happen on one subnet alone uh what was the need for us to listen to multiple uh subnets uh I mean specifically for the aggregator
Gajinder Singh: So I can be a super aggregator right. I can say that I can aggregate I have a big heavy machine and I can aggregate three subnets, four subnets. I can aggregate all of them,
Parthasarathy Ramanujam: Okay.
Gajinder Singh: right? So there will be such kind of machines in the network which will like
Parthasarathy Ramanujam: Okay.
Gajinder Singh: like in Pas we have super nodes already there, right? So which are helping the network juggle and that is what my assumption is that this will happen
Parthasarathy Ramanujam: Okay. Okay.
Gajinder Singh: and uh it's a good thing that aggregator is not bound by the validators that are attached to it. There could be no validators at all. You can just purely run an aggregator by as an totally intrusive uh
 
 
00:11:10
 
Parthasarathy Ramanujam: Right. Okay.
Gajinder Singh: thing.
Parthasarathy Ramanujam: So I think then the problem was running in I think sorry
Anshal Shukla: So we can limit the number of Yeah,
Parthasarathy Ramanujam: uh I was saying that yeah
Anshal Shukla: I was saying that can we limit the number of uh subnets that we listen to as an aggregator?
Parthasarathy Ramanujam: yeah that that that was my question. So, uh,
Gajinder Singh: So,
Parthasarathy Ramanujam: I think because I'm running on
Gajinder Singh: so that is that should be machine dependent right. So if your machine can take it then you will you will put on a higher number.
Parthasarathy Ramanujam: a
Gajinder Singh: Question is that if we have a higher machine can we still solve the problem right? Can we still be an aggregator? should not basically it should so your aggregator role should scale with uh the machine slash number of cores slashmemory that you have and if that is not happening then it's a problem that means
Parthasarathy Ramanujam: correct. Yeah.
Gajinder Singh: that there is some other bottleneck that we need to figure out uh but if it's failing then you will probably
 
 
00:11:57
 
Parthasarathy Ramanujam: Yeah. So,
Gajinder Singh: decide on the basis of machine how many subnets you want to aggregate
Parthasarathy Ramanujam: so that's fine. So, I'll uh what I'll do is right now uh what uh the lean quick start does is by default it subscribes all aggregators to all subnets but we are running on a 8 uh core 16 gig uh
Gajinder Singh: But we decided that it will be one subnet per aggregator,
Parthasarathy Ramanujam: machine.
Gajinder Singh: right? So why are we subscribing and aggregator all
Parthasarathy Ramanujam: Yeah. No,
Gajinder Singh: subnets?
Parthasarathy Ramanujam: when I say listening not uh so the aggregator alone subs uh listens to aggregations from other subnets as well.
Gajinder Singh: No, it should not. It should not do that right.
Parthasarathy Ramanujam: It shouldn't right.
Gajinder Singh: It is only as a power or super aggregator that will do that. But this is not what we want to test.
Parthasarathy Ramanujam: So that
Gajinder Singh: We want to test one aggregator per subnet,
Parthasarathy Ramanujam: that
Gajinder Singh: not one aggregator listening to and aggregating all the
 
 
00:12:48
 
Parthasarathy Ramanujam: right so that that was the configuration change which I've corrected right now.
Gajinder Singh: subnets.
Parthasarathy Ramanujam: Regardless of that, we are having issues when um one um Zam client functioning as an aggregator listens to over uh 50 or 60 peers and starts receiving um um uh I mean gossip uh related stuff. So probably it for now it makes sense for us to limit the number of peers a particular uh node can receive subscription to. Is that something you want want to uh do because otherwise uh the aggregated clock starts to fall behind? Uh until we resolve that problem, we should add some kind of a
Gajinder Singh: So no the limit is limit is something that you
Parthasarathy Ramanujam: limit.
Gajinder Singh: configure right. So there is no limit to be added as such.
Parthasarathy Ramanujam: Now, uh I've noticed on beacon clients, they have max gossip clients ma um kind of a parameter, right? They don't peer for more than 30 clients or something of that
Gajinder Singh: But we don't have so many clients.
Parthasarathy Ramanujam: sort.
Gajinder Singh: This is very small number but I essentially get what you what you are saying we should have target peers but their target peers run in 200s for example right.
 
 
00:14:01
 
Gajinder Singh: So that is the target that we want to have and this is such a small target as of now but
Parthasarathy Ramanujam: Okay.
Gajinder Singh: eventually yes about 200
Parthasarathy Ramanujam: Okay. So what I'll do is uh I so I I'll make the change on lean quick start to
Gajinder Singh: for
Parthasarathy Ramanujam: ensure uh each uh subnet uh I mean each aggregator listens only to its own subnet and not uh others. And uh there there is also the other
Gajinder Singh: yeah by default. Yeah, by default just make this change and if someone wants to test their multi-agregator
Parthasarathy Ramanujam: Yeah.
Gajinder Singh: functionality because we do want to test it. So we will we can basically edit it locally and test it
Parthasarathy Ramanujam: Okay.
Gajinder Singh: out.
Parthasarathy Ramanujam: And can I also have a review on PR886 which is um I've introduced a few new um metrics parameters just to determine what is causing uh delay because I noticed in few cases uh the main thread was blocked for nearly u uh I think up to one or two minutes.
 
 
00:15:02
 
Parthasarathy Ramanujam: So I I need to understand where the lock is uh or what's causing that delay. So if we can figure that out in the next long run, we'll be able to uh make further changes and improve the
Gajinder Singh: So I I did review it and it looks fine to me.
Parthasarathy Ramanujam: performance.
Gajinder Singh: I mean as such on a high level. So if someone wants to give a deeper review and approach is fine.
Parthasarathy Ramanujam: Okay, sounds good. So Anchel or Kai if you can please have a look at that and let me know. I'll u if you are happy I'll merge this and then we can run our uh uh devet again with the corrections uh configuration corrections and then we should have better view of what's going on. Yeah, that that's me. No further points.
Gajinder Singh: All right. And uh so so with these if you think that there are new metrics that you introduce and they should be upstream. So that is another thing that we are always looking for what is that we can add to the spec right in terms of metrics as well.
 
 
00:16:06
 
Gajinder Singh: So do that and as well as uh for my
Parthasarathy Ramanujam: Yeah.
Gajinder Singh: for the PR that I got through Z clause logo destation subnet coverage uh I think the metrics that would be there as well we so I asked it to add metrics for all those things and uh for all the coverage uh labels that I added that I got it to add and uh it does that I don't know it has processed my command but I have asked to see why it is not
Parthasarathy Ramanujam: Mhm.
Gajinder Singh: processing uh why zclaus is not automatically processing the command because it is supposed to pick up run a uh uh run a poll and every now and then basically process all the commands that are being made on the on the zclaw PR or basically anything that is addressed to zclaw uh so that needs to be checked Or did you manually ask Zcloud to to basically
Parthasarathy Ramanujam: Yeah,
Gajinder Singh: apply the changes para because from the commands it seemed like it processed
Parthasarathy Ramanujam: I uh I did ask it
Gajinder Singh: it?
Parthasarathy Ramanujam: uh for a couple of as but the latest one I don't know.
 
 
00:17:23
 
Parthasarathy Ramanujam: Uh I think you're referring to H76, right? Uh yeah,
Gajinder Singh: Yes. 876.
Parthasarathy Ramanujam: that uh it has done. I manually asked it to do it. Uh but the latest one that you requested it is not done that
Gajinder Singh: Yeah,
Parthasarathy Ramanujam: yet.
Gajinder Singh: it has not done because I mean the poll is not working I guess.
Parthasarathy Ramanujam: Yeah.
Gajinder Singh: So I have asked to check why the poll is not working
Parthasarathy Ramanujam: Yeah. Okay.
Gajinder Singh: anymore. Okay. All right. And what what is the plan to go to 128 validator as I I want to see that very fast. I mean, I want to see us doing at least few runs before next week's
Parthasarathy Ramanujam: Yeah. So, uh I have one with 64 validators right now.
Gajinder Singh: call.
Parthasarathy Ramanujam: I increased uh requested an increase in Kota which Hexner has honored. Uh I just need to provision the machines right now. Uh prepare it for it and then I can start uh scaling up.
 
 
00:18:24
 
Parthasarathy Ramanujam: Uh I I can do that over the weekend or before next call. We should have something uh uh ready.
Gajinder Singh: And with regard to so so this is unible so people are trying to come up with tooling to run the same machine is our local tooling not working is my
Parthasarathy Ramanujam: No, it is working. it is uh you can use the lean quick start even with local tooling for running multiple subnets. I don't know why uh they prefer the latest one that I did uh parallelizes uh the deployment as well but I don't know why they prefer uh I mean Kubernetes or something else rather than this.
Gajinder Singh: Yeah,
Parthasarathy Ramanujam: So
Gajinder Singh: Kubernetes on I mean Kubernetes and uh Cottos is they don't really make sense to me at all. So I don't know what's going on over there. Maybe they have free time or free funds on their hand to chase after something that is already there and have no utility. But let's see. Okay. Uh so if they come up with the better tooling, we don't mind.
 
 
00:19:34
 
Gajinder Singh: We will definitely use whatever is the best in the ecosystem. And so we do encourage but it should be definitely better than the tooling that we have right not just a pure replica of
Parthasarathy Ramanujam: Yep.
Gajinder Singh: it. Okay let's go to Kai
Kai Chen: Uh last week, last week first I uh raised the PR move move the aggregate aggregate aggregation uh uh from uh libcv thread to the chain work thread. Yeah. So uh it it seems it uh much time time cost when I do some uh microbenchmark. uh and uh and in that PR I also I also changed some configuration about uh the zig uh zig thread pool uh uh thread numbers. So because um the the aggregate aggregate FFI library which uh uh because it's it it's uh developed in Rust it it it already used the Rust ray on thread pool. So, so right now we we have double times thread number numbers. So, uh in that PI I just uh uh separate and reduce some thread numbers.
 
 
00:21:36
 
Kai Chen: So, but I'm not sure um is it is it bad. So maybe uh when plas are uh testing in in the that night uh maybe we we we need uh observe some methods. Maybe uh further we can we can change in that configuration further.
Gajinder Singh: So, what exactly is the configuration change that is talking about?
Kai Chen: Uh right now right now right now right now there is there is a not uh uh there is there is a not uh configuration uh right now the zig thread port number is used uh CPU uh the machine CPU number min uh uh minus plus minus minus three. Yes. So because we uh we have uh one uh lib XV thread and uh one uh rasterly PP lightwalk thread and one chain ch uh chain work.
Parthasarathy Ramanujam: screen worker.
Kai Chen: Yeah. So, so,
Parthasarathy Ramanujam: Yeah.
Kai Chen: so I just uh change change the change the reduce the numbers for for the Z uh Z thread plan and also set uh uh that array on raster thread numbers.
 
 
00:23:31
 
Kai Chen: So right now we be uh the all thread numbers maybe totally uh equal the CPU uh number
Gajinder Singh: So I I don't think we need to so fine grain control it. So wherever you need threads at least span the amount of cores or CPUs that we have because even if you span you span more threads than the number of CPUs that you have basically CPUs will automatically schedule it properly so that uh you should be able to get more likely the amount of whatever is the free CPU uh efficiency that that is out there. So you should you should be able to squeeze it out. So I'm I'm not sure whether doing
Kai Chen: Yes. Yes. So maybe we uh so pass up maybe you you can you can change it uh
Gajinder Singh: Yeah.
Kai Chen: you can change it that's maybe change back so you can obser observe the the uh running uh yeah in in the in test net uh
Parthasarathy Ramanujam: Yeah,
Kai Chen: in a net. Yeah.
Parthasarathy Ramanujam: sure. Uh,
 
 
00:24:53
 
Kai Chen: So
Parthasarathy Ramanujam: so I I haven't yet run um uh after you your PR was merged. So if there is regression, I'll probably revert your changes and then go back to what we were uh using before. Um and then see uh where we are.
Gajinder Singh: So what I understand we were not using any threading in the chain workers right it was just a
Kai Chen: Okay.
Parthasarathy Ramanujam: That's right. There was just a single Yeah, there was a single train
Gajinder Singh: single so it should at least now if there
Parthasarathy Ramanujam: worker.
Gajinder Singh: are multiple threads it should be better than the last time right and yeah so I I think instead
Parthasarathy Ramanujam: It should Yeah.
Gajinder Singh: of uh trying to you know do num CPU minus 3 or something like that. Just span equal to num CPU threads and the uh operating system is supposed to take care of it right.
Parthasarathy Ramanujam: Yeah.
Gajinder Singh: Okay. So run with Kai ch with K changes and if uh there is improvement again run with numbum CPU threads everywhere and there shouldn't be much degradation.
 
 
00:25:58
 
Parthasarathy Ramanujam: Yeah.
Gajinder Singh: If there is no degradation then it means there is degradation hopefully improvement and uh so let's just keep things simple and not try to have fine gear control over number of threads that we span just blindly span according to num blindly span as per num CPUs that you have anywhere that you need it even if your total thread pool might
Parthasarathy Ramanujam: Sure.
Gajinder Singh: exceed uh two or three or 10 times the number of CPUs it does not actually matter
Kai Chen: Yes. Uh other and I I arrested three PRs for fix uh uh five uh test uh failed cases. Uh I already test them in my local. So um if if you can approve it
Gajinder Singh: So para are you looking into them as
Parthasarathy Ramanujam: Yeah. Yeah. I'm uh I'm looking at the hive uh related one.
Gajinder Singh: well?
Parthasarathy Ramanujam: Uh I'd uh uh I mean finish the review after this uh call and then uh we can merge that.
Anshal Shukla: Yeah, I was also looking into that. So,
 
 
00:27:16
 
Gajinder Singh: Come.
Parthasarathy Ramanujam: Okay.
Anshal Shukla: uh Kai in that we have uh mentioned uh we have tags like cache is equals to false for a lot of r stuff.
Kai Chen: Yes. Yes.
Anshal Shukla: Do we need
Kai Chen: That Yes. I I also asked the Zclaw why why it add that
Anshal Shukla: it?
Kai Chen: in CI and yam. I think it already uh it have a comment. I I I will send you to
Anshal Shukla: Okay.
Kai Chen: you.
Anshal Shukla: Can we try changing it and see if the CI works
Kai Chen: Uh,
Anshal Shukla: fine?
Kai Chen: okay. Yes. Maybe it's not is um maybe it's not needed.
Gajinder Singh: All
Kai Chen: It it it seems just just
Anshal Shukla: Yeah.
Kai Chen: optimization.
Parthasarathy Ramanujam: So uh Kai one uh sorry uh Kai question is how how do you interact with
Gajinder Singh: right.
Parthasarathy Ramanujam: uh Zcloud via DMs? Uh do you specify the model uh which should be used?
Kai Chen: No, no. I just uh I just uh uh re uh raise some uh issues and uh and give some information to it in the issue
 
 
00:28:55
 
Parthasarathy Ramanujam: Okay.
Kai Chen: description.
Parthasarathy Ramanujam: Uh because you can specify which model uh Zclaw should use uh in order uh for you. I mean for different task you can use different models. Uh so for example if you want it to work with opus you can specify slashmodel and then um ask it to work with that particular model. Uh nupush sent me I'm not sure if she shared with the group. I'll I'll share that as well. You can uh I mean we can't do it in the group but if you want I'll do I'll forward what her message was to
Kai Chen: Oh yeah, that's that's good.
Parthasarathy Ramanujam: you.
Gajinder Singh: So we can do it in the group. I mean it does not stop us from sharing in the group. Apart from that uh ask no if the same thing works in
Kai Chen: Okay.
Gajinder Singh: issues/comands as well because I think that is that could just directly be for uh telegram prompts. So ask Nupur if the same thing works even for description
Kai Chen: Okay.
 
 
00:29:54
 
Gajinder Singh: issues uh and comments and if not basically ask her
Parthasarathy Ramanujam: Okay.
Gajinder Singh: to configure it so that it works there as
Parthasarathy Ramanujam: Okay, I'll do
Gajinder Singh: well.
Parthasarathy Ramanujam: that.
Gajinder Singh: But I think yes even if you have made an issue you can just ask on telegram to process it through a particular model uh then it will directly do that. if until the point where we also get it working directly through issue itself. But yes, I think uh there is nothing hidden. So we can publically post in and pin it as well. I guess in Zclaw channel itself we can post and pin
Parthasarathy Ramanujam: Okay,
Gajinder Singh: it or in main
Parthasarathy Ramanujam: I'll do that thing. Uh,
Gajinder Singh: frame.
Parthasarathy Ramanujam: I think Zclaw is better. I mean, that's fine. I mean, either thing is okay.
Gajinder Singh: Yeah, doesn't matter. Yeah, whatever is most handy for everyone is okay. All right. Uh coming to my update, I have been again reviewing PRs. I was also doing some beam runs and uh I
 
 
00:31:06
 
Kai Chen: Thank you.
Gajinder Singh: have was I I did a PR asked Zcloud to do a PR to add some to show some aggregate coverage reports. Uh and probably also add metrics for it and hopefully then we can upstream those metrics to leanspec. uh and I will basically continue running uh Z to Z node uh Z to Z network and uh figure out what is going on and if there is anything that is uh that is something that we need to fix or we need to put out more information in our logs uh and we'll continue debugging I I will also look at uh DevNet 5 PR that rental made and uh go through it and see if we are all on the same page regarding the DENTE 5 and uh how we are doing a single block signature and reusing it again. So with all these things I think uh uh we are at a good point. We just need to keep improving the performance of Z aggregator and make sure that uh none of things happen on hot paths. So for example uh even though we we start the aggregator right on uh on the on the particular interval then we need to make sure that aggregator is running within a particular timeout and is running on a separate thread and not li xcv thread.
 
 
00:32:59
 
Gajinder Singh: So this is something that we need to also make sure uh so I'm not sure whether this has already been addressed or not.
Parthasarathy Ramanujam: Uh I'll look into it after the current PR is merged. Uh just validate whether it's working as per your expectation.
Gajinder Singh: or also put a chain worker on it. I don't know because we can also do par aggregations right we can do a tree kind of an
Parthasarathy Ramanujam: Yeah.
Gajinder Singh: aggregation so anchil
Parthasarathy Ramanujam: So yeah,
Gajinder Singh: uh can you look into this as well to optimize the aggregation further
Anshal Shukla: Yeah. Yeah.
Gajinder Singh: and uh
Parthasarathy Ramanujam: Will
Anshal Shukla: So I have plans to do that and I want to like revisit the whole uh threading architecture. Uh I was just busy with the DevNet 5 PR. It took more than more time than I had expected. Also like I wanted to add upon like the splitting of signatures. So right now I have how I have implemented is that when on block is called and a particular node is an aggregator it basically looks into the block block and sees that if there's any new uh attestation bits that are there in case there is an additional attestation bit it basically deconstructs it and reconstructs a proof and uh propagate it via gossip.
 
 
00:34:21
 
Gajinder Singh: Yeah, but that should not sort of block the interval.
Parthasarathy Ramanujam: Okay.
Anshal Shukla: Yeah,
Gajinder Singh: I mean it should happen in a parallel thread
Anshal Shukla: it's it's Yeah, it's
Gajinder Singh: and in the block proposer.
Parthasarathy Ramanujam: Okay.
Anshal Shukla: async.
Gajinder Singh: Then you also need to wait that if you started that and if it's not done then your block proposer might not be able to propose because it will not get it might not be able to uh get the same justified or finalized right that you might have got in the block on a separate uh
Anshal Shukla: So it's it's not async in in the block proposal path. It is async in the aggregation path. In the block proposal path, it has to merge all those signatures and the proposal signature to create the block proof. So it's not async there
Gajinder Singh: Okay. If it's not a sync but we still need to the we still need
Anshal Shukla: because I thought
Gajinder Singh: to did we apply an assert where whenever we are
Anshal Shukla: that
 
 
00:35:22
 
Gajinder Singh: constructing the block we are at least getting the same justified and finalized that we have seen on the fork choice
Anshal Shukla: Yep. So that is there. Uh I think that
Gajinder Singh: that is there because if if that is there and if you don't get same justify and finalize your block proposal will fail, right? Okay,
Anshal Shukla: Yeah.
Gajinder Singh: just uh reverify this and make and basically we need to figure out uh what to do with the block proposal if we had votes on a separate fork that moved the justifier or finalized and we have not been able to split and imported it by the time the proposal comes up.
Anshal Shukla: So as a block proposer I am not doing that right now.
Gajinder Singh: So
Anshal Shukla: I am just relying on aggregator to do the splitting and submitting the updated payload to me via gossip.
Gajinder Singh: okay. Okay.
Anshal Shukla: Yeah.
Gajinder Singh: So the so you are saying that this is a job of aggregator to do that. So when when the block proposal is coming and you see there are new bits that you don't
 
 
00:36:30
 
Anshal Shukla: Yeah.
Gajinder Singh: have. So you don't do anything at that point. You wait for some aggregator and if you have that aggregator to do that job at that particular point.
Anshal Shukla: Yes.
Gajinder Singh: Okay, that makes sense. And but what you can also do is that if you are an aggregator then uh instead of waiting for example at least one more interval you can start the splitting early on
Anshal Shukla: As soon as I receive a block uh so I have added this logic on the onb
Gajinder Singh: right.
Anshal Shukla: block function itself it basically checks if there's a new and it's like a async function so it will continue to run asynchronously.
Gajinder Singh: Right. So the question is that if we are not able to do the job till the point I guess you will be window and aggregator will will not do anything.
Anshal Shukla: Mhm.
Gajinder Singh: It will
Anshal Shukla: Okay. I am not skipping it right now,
Gajinder Singh: just
Anshal Shukla: but yeah, I'll add that logic because it basically means that I'm not in sync with the current chain and I should like just escape the proposal itself.
 
 
00:37:47
 
Gajinder Singh: not skip the the proposal will be skipped if if you are for example not able to get the justified and the finalized that is in folk choice. So you don't need to skip the proposal specially for it because unless you unless the justified and finalized moved uh and you your latest proposal does not has that then only it should be skipped otherwise it's fine I
Anshal Shukla: Okay, got it. Okay,
Gajinder Singh: guess
Anshal Shukla: I'll add this logic. Uh, I'll verify if I I I don't think I did that. So, I'll just
Gajinder Singh: you just verify it on the code flow and see if this is the behavior that will be
Anshal Shukla: verify
Gajinder Singh: there and I think uh maybe we can add some spec test uh or hype test that basically are in accordance with this particular
Anshal Shukla: Okay.
Gajinder Singh: behavior.
Parthasarathy Ramanujam: Uh one question uh Gajender the uh discussion during Wednesday's call on in inclusion of execution layer uh into DevNet is that planned from DevNet five or X.
Gajinder Singh: I don't think uh it's devet 6 uh but I think devet five testing will be long running and it will be a while we'll have definite 6 but yes it's definite
 
 
00:39:11
 
Parthasarathy Ramanujam: Okay. So,
Gajinder Singh: 6
Parthasarathy Ramanujam: no change required on um the tooling for that anyway right now. So, just wanted to plan that.
Gajinder Singh: yeah as of now no touring change required but DevNet 6 if we want to take lead on it as soon as we do DevNet 5 we can start working on DevNet 6 spec uh because I think it's better to take the lead on it
Parthasarathy Ramanujam: Okay,
Gajinder Singh: uh and keep others chasing us as it has been as we have been doing before.
Parthasarathy Ramanujam: sure.
Gajinder Singh: Uh so that will be a good idea even though I think that dev next spec also might take longer because a lot of things will get introduced now. Uh but also definite 5 testing needs to be on a bigger scale and more thorough. So things will stay definite 5 for a while is my estimation. But again if we have a DevNet 6 proposal spec proposal then it is it will be to our advantage that we are thinking
Parthasarathy Ramanujam: Okay.
Gajinder Singh: ahead.
Parthasarathy Ramanujam: So, uh sorry, Angel, the quick question is for DevNet 5,
 
 
00:40:20
 
Anshal Shukla: Okay.
Parthasarathy Ramanujam: we still have this PQ heartbeat. Would that require a tooling change then?
Gajinder Singh: No dev the PQ heartbeat is out of devet 5.
Parthasarathy Ramanujam: Oh, it's not. Okay,
Anshal Shukla: Yeah,
Parthasarathy Ramanujam: this
Gajinder Singh: It's also out of
Anshal Shukla: I was coming to that.
Gajinder Singh: six.
Anshal Shukla: So if you if you have like PQ heartbeat for DevNet 6 then we'll push the
Parthasarathy Ramanujam: Okay.
Anshal Shukla: execution pro uh execution integration to DevNet 7 or will we try to merge
Gajinder Singh: So I think uh from what I understand I think uh let's do execution
Anshal Shukla: it?
Gajinder Singh: integration first and then do pick your heartbeat unless we have a clear proposal in mind which I have not dig through properly and maybe I'll have
Anshal Shukla: Okay.
Gajinder Singh: it if I go through it and put some cycles into it but
Anshal Shukla: Mhm.
Gajinder Singh: yes I I think for now I guess net 6 could be execution integration
Parthasarathy Ramanujam: So we need to define the entire engine API uh specification or whatever we need or
 
 
00:41:30
 
Gajinder Singh: it should be easy but uh we can just do SSD engine API I guess I mean engine API is not really a big deal it's quite easy but we might
Parthasarathy Ramanujam: Okay.
Gajinder Singh: not be able to do engine SSD engine API which basically depends upon if execution clients support it or not right that is also
Parthasarathy Ramanujam: Right. So, uh we we will only be using the latest uh uh folk choice uh engine,
Gajinder Singh: there.
Parthasarathy Ramanujam: not the previous one. We don't need to support the previous uh thing, right? Yeah. Uh, I
Gajinder Singh: Uh so so we can say that okay this is the fork on
Parthasarathy Ramanujam: mean,
Gajinder Singh: which of the engine that we are doing it. idea is to yes keep uh keep us up to date and in sync and in step with whatever is the latest folk on the engine API on the execution.
Parthasarathy Ramanujam: right. Mhm.
Gajinder Singh: So basically we are always ready. So I so in that s in that sense I don't think there is any any blocker in the current execution that we have we might not be able to integrated without having some specific consensus beacon consensus feature because we'll be able to implement that in the lean consensus lean consensus itself right so in that sense I think it should not be a blocker and we we will just try to be in sync with the latest execution. But yes, we will go fork by fork uh on that and say this definite is
Parthasarathy Ramanujam: Okay.
Gajinder Singh: on this particular fork and maybe on some later devet we will increase we'll bump up the fork on the execution as well. If there is a bump on execution fork and we can easily upgrade it.
Parthasarathy Ramanujam: Okay.
Gajinder Singh: All right. Anything else we have to discuss?
Parthasarathy Ramanujam: Nope, not
Gajinder Singh: All right.
Parthasarathy Ramanujam: really.
Gajinder Singh: So let's do DevNet 5 implementation. Let's do DevNet 4 uh high validated account. Let's make sure one aggregator per subnet per client. Uh and yeah, let's continue to optimize uh Z aggregator and Zimo paths. Yep, that is what we should be continuing to do.
Parthasarathy Ramanujam: Sounds
Gajinder Singh: All right guys,
Parthasarathy Ramanujam: good.
Gajinder Singh: uh thanks for being in the call and we'll see you on May 22 and before that on next the call.
Anshal Shukla: Okay, thanks.
Parthasarathy Ramanujam: All right. Thank you. Thank you.
 
 
Transcription ended after 00:44:37

This editable transcript was computer generated and might contain errors. People can also change the text after it was created.
