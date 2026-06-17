Jun 12, 2026
Zeam call - Transcript
00:00:12

Gajinder Singh: Hello everyone. Let's start uh our call. Today's June 12 and we'll discuss our progress on DevNet 5 uh issues we are facing and the resolutions we are doing and the status of our DevNet 5 runs. So let's start with Parthononet.
Parthasarathy Ramanujam: Um so yeah thanks Gajender. I uh started testing DevNet 5 uh from Monday. Uh ran an all uh Zam Devnet with four uh subnets I believe and uh it was pretty stable. Uh justification and finality continued for over 24 hours without any issue. at least on uh the chain was progressing as expected. Uh after around 2,000 slots or so, there was a slight wedge but uh the network recovered. Uh but uh as Gender pointed out in that chat, there were few issues or smoking guns that we saw where uh the aggregation uh or rather the block recursive proof generation was uh uh a bit delayed and taking longer than it should. Uh so I stopped uh the devet um after a while and I haven't restarted again.


00:01:39

Parthasarathy Ramanujam: Yeah,
Gajinder Singh: just to correct over uh you over there.
Parthasarathy Ramanujam: sorry.
Gajinder Singh: So block generation was not taking time. So we were generating the block so in time but then we were not able to publish it in time. So, so from the log it seems that at many times at most of the times the block generation was on
Parthasarathy Ramanujam: Yeah, but uh looking at the latest metric that Kai shared,
Gajinder Singh: time.
Parthasarathy Ramanujam: it appears that uh the stark generation itself uh is longer. It takes around uh uh 4 seconds, I think. I don't know. That's why we were publishing probably in the next uh slot. I'll let Kai correct it. I mean, we've added a new metric,
Gajinder Singh: I mean we've added it I mean even
Parthasarathy Ramanujam: but I have to run with that.
Gajinder Singh: on the sim runs basically I'm I'm able to see that publishing is delayed by the generation so maybe generation is delayed but then publishing is then even further delayed right so first of all that needs to be fixed that why even when we generate the block in


00:02:37

Parthasarathy Ramanujam: Yeah.
Gajinder Singh: time we are not publishing in time so maybe it's related to threading uh what we are doing over there I think we are basically blocking doing a blocking weight and before publishing and uh we are generating the block on a thread right so I'm not sure what exactly is happening but my question is why is it happening and then what is the resolution for it so maybe first talk
Parthasarathy Ramanujam: Yeah. So, uh we need two
Gajinder Singh: about this particular issue so kai if do you have any information further information
Parthasarathy Ramanujam: metrics.
Gajinder Singh: on it or a resolution for And then we can come back to other issues.
Kai Chen: Um, so I I I send that that's uh in the TG group. So for produ for the log produce block only include that select type one ready. So it it it's super fast. So it not include the the
Gajinder Singh: So can can we include can we also do an info logo where we say that we have
Kai Chen: um
Gajinder Singh: basically been able to create the aggregate proof because yes you might be correct that uh the aggre proof aggregation is happening later on after the block production.


00:04:18

Kai Chen: Uh I so uh PA suggest uh add a new matrix. Yes. Uh so so the problem so the problem uh uh observed is that the uh stock merge it is slow. it uh it can only so uh it can only works when it has uh one uh type one testation I think.
Gajinder Singh: All right. Right. So I see that the PR996 is to add the metric.
Kai Chen: So
Gajinder Singh: Um just checking whether you also added a log for it because metric is okay
Kai Chen: yeah.
Gajinder Singh: but log is also quite handy to
Kai Chen: Yeah.
Gajinder Singh: debug
Anshal Shukla: So K right now uh we have like a 50% uh slot interval for
Kai Chen: Okay.
Anshal Shukla: type one aggregation right
Gajinder Singh: on on the beam sim you have almost like 700 millisecond right so in first 80 millcond itself the block gets produce the first part gets produced where we log it out uh so I'm seeing in chain where we have this I'm seeing that build block proof so when we are calling build block proof can


00:05:46

Anshal Shukla: Oops.
Kai Chen: See?
Gajinder Singh: we basically do a log that going for aggregated proof production or something like that So I see that there is a new log that says that type two block proof start merge and give some info regarding that. But also let's put a log where it's clear that this function is being called by the caller.
Kai Chen: Okay. Okay. So yeah.
Gajinder Singh: Just add this extra log line wherever build block is default.
Kai Chen: So from uh from my testing I think right now only only 20 to 30 uh percent percentage can can uh can be published in internal zero. So almost 70 and uh above 70 uh publish delayed
Gajinder Singh: So in so my question is that then what is the configuration that is needed so that let's say
Kai Chen: to
Gajinder Singh: 90% of that is publishing time. So let's also try to run it on a bigger machine or machine with more cores and see that what is what is that configuration that gets us over there because then the question will be optimization and the hardware requirements.


00:07:28

Anshal Shukla: Yeah. So, Gajinda like uh I had added a uh slag which was like the percentage
Gajinder Singh: Um,
Anshal Shukla: of the block production interval that has to be utilized for type one proof aggregation. It was earlier set to 90% because we just had like type type one single message aggregation but right now we have uh multi- message aggregation as well. So I think I had asked try to default it to 50 but even if like if so
Kai Chen: Happy.
Anshal Shukla: we deal all lot like 50% of the time for uh for single message aggregation and the rest 50% of the uh time for type two aggregation multi- message aggregation. So if we are if like 70% of the blocks are still missing the uh their block production interval in that case we we should like further I think we can try further putting it down and see because there's a explicit condition that it should at least have a single aggregate that should have completed the type one aggregation post that anything midway mid-flight will be rejected. Uh so uh I think right now how Kai had done it and I didn't push back on it because we had to see like if the how long the type two aggregation is taking but once we have those numbers in place we can again move back to like the earlier


00:08:53

Anshal Shukla: configuration where I was I had added a time time out. So if we we aren't able to uh aggregate even the type two within like the 10% boundary of the block interval we should basically skip the production itself and just resume the normal validator due to instead of like doing a late publishing and which will further like it will make the folk choice a little bit uh fogged which is not the ideal case if we aren't able to produce in like 900 second. So 800 is like ideal but I think we can have some buffer. So if you aren't able to we can have it like configurable 10% 20% thing. If you aren't able to config uh produce it in like 10% of the block production interval we should just skip the production itself.
Gajinder Singh: So I might not agree with that because for example even with this you know delayed block production for example we see justification and finality right so so


00:09:56

Anshal Shukla: Mhm.
Gajinder Singh: we it is like okay you know we don't need to do that we can still continue to run but
Anshal Shukla: Yeah.
Gajinder Singh: I would like a log that the block production is delayed uh or could not be completed in this interval when we are switching to another to the second interval. Uh so this is something this is a log that I would like as well as a metric on it uh like para wants and uh if we have that then basically our aim should be again to figure out how to best optimize type two and how to make sure uh that we have the right kind of machine for doing this kind of uh testing right so so on both the fronts
Anshal Shukla: So I want to have a I want to have like a delayed uh delayed interval where we like stop the block production itself because that way we can start accumulating a lot of work for the production blocks and that way like suppose there are four pending blocks that are trying to do type two aggregation but none of them complete because we we say that we'll anyways public plates but because of that it will be like all of them will be starving the resources


00:11:07

Gajinder Singh: So as I discussed in the call I think we can shift type one to the aggregation to the last interval of the previous slot right we know that next slot we as a proposer
Anshal Shukla: Mhm.
Gajinder Singh: so uh even though everything needs to be done but we can still start aggregating right uh we so even without basically trying to figure out what things to pack into the block because for the new aggregates if they are on the same data we can we can aggregate them anyway.
Anshal Shukla: So you're saying even for the validators they'll start aggregating across the subnets that they have received from the
Gajinder Singh: And
Anshal Shukla: aggregator.
Gajinder Singh: yeah and basically then we determine on the basis of uh how many data types we have that we will only pack this much. Basically we'll say that okay or or we say that okay type one aggregation as you said will not exceed this particular window and whatever then we'll have we go with it.
Anshal Shukla: Yeah.
Gajinder Singh: Uh the thing with that is that sometimes you might need to pack uh test stations from your parallel branch


00:12:18

Anshal Shukla: Let's see.
Gajinder Singh: in which you might so for that special cases where a side branch move forward justification and finalization you need to pack that because otherwise your block proposal will al always fail even if you don't if you don't pack any of those things.
Anshal Shukla: Yeah, but that
Gajinder Singh: So for the specialized cases for the specialized cases we will
Anshal Shukla: happens.
Gajinder Singh: basically allow the block production to go on for a longer time because if we don't do it if someone else don't do it the network will eventually stall right so someone has to complete that block production or it needs to be figured out by the way of uh the additional splitting aggregator role that we added
Anshal Shukla: Yeah. So for the side branches whenever a block is received we already do the splitting as soon as we receive the block itself. So for the relevant attestations we do the splitting at that moment itself and write it to the aggregations map I think to the new payload aggregation map. So that is something which is already being taken care of beyond the proposal


00:13:25

Gajinder Singh: Who is who is doing the splitting?
Anshal Shukla: interval.
Gajinder Singh: All all the nodes are doing splitting or only the aggregators are doing all the
Anshal Shukla: Yeah. All the notes I think all
Gajinder Singh: nodes.
Anshal Shukla: the
Gajinder Singh: Okay.
Parthasarathy Ramanujam: And can we do that only on the aggregator?
Gajinder Singh: So,
Parthasarathy Ramanujam: Uh if that's time consuming because aggregator is a larger machine.
Gajinder Singh: so the problem with that is basically if an aggregator doesn't do it and you don't get it, your proposal will fail, right? As a proposer, you would want to do it yourself because as a
Parthasarathy Ramanujam: Okay.
Gajinder Singh: proposer, even if you don't pack anything, you can still ment block, right? Even if you don't get aggregates, you can still mention and finality. If it's not moving justification and finality, it doesn't matter then. So it becomes a necessary condition. So unshel I think what we can do is basically only in the case the side branch is moving justification and finality we do that otherwise we hope that some aggregator will do


00:14:32

Anshal Shukla: Yeah, I think that is the case right now.
Gajinder Singh: it.
Anshal Shukla: But I'll just verify uh maybe Kai if you remember you can tell us but I think that is the case right now as well
Gajinder Singh: Okay.
Parthasarathy Ramanujam: Uh which is the more time consuming
Anshal Shukla: but I'll
Gajinder Singh: So, do we have do we have Yeah,
Parthasarathy Ramanujam: operation?
Anshal Shukla: anyways
Gajinder Singh: it should be a timeconsuming
Parthasarathy Ramanujam: Sorry, no. Which among these is time consuming? Aggregation of type one or type two or mix of both?
Anshal Shukla: so I think type two is more time consuming.
Parthasarathy Ramanujam: Thank you.
Anshal Shukla: Uh splitting is also a bit time consuming.
Parthasarathy Ramanujam: I'll
Anshal Shukla: Uh I think type one aggregation is not that time consuming
Parthasarathy Ramanujam: help.
Kai Chen: Yes.
Anshal Shukla: right now with the recent fixes that Mile has made. So yeah, that's like I that's like my rough understanding. I don't have numbers to back it.
Parthasarathy Ramanujam: Okay. I mean I'm just wondering if we can add a specific metric to see uh within our clients which among these is uh consuming a lot of time for us and then we can implement uh I mean uh improvements based on what is the the actual scenario that's happening are we uh I mean is the tail due to uh a delay in processing of type one or type two or a mixture of both and then we can apply strategies This


00:16:01

Gajinder Singh: Yeah. So have we added metrics for it is the first question that if yes also have we added log for it when this thing happens and of course when we do that in the log itself can we also log out how much time it took because again log is quite handy to figure out many things. So until uh do we have metrics and log for
Anshal Shukla: I don't know. I haven't seen into that.
Gajinder Singh: this?
Anshal Shukla: I thought like K and P was looking guys. Truly Hammet.
Gajinder Singh: Okay. So uh Kai did a guy did a PR for adding metrics to the normal block type to production uh merge proof production and he will add a log and then we can merge that PR and then the Windows PR that okay this is the window in which we want type one to have finished with a timeout and then uh type two will start and then a third PR which basically will start type one aggregation on the previous slot itself. So, so who's going on the going to do these two PRs after the KPR is merged?


00:17:27

Gajinder Singh: because I don't think there is anything else in left hand apart from putting a log.
Anshal Shukla: I can do that. So it's just like adding the logs and metrics for the time that it takes and uh when it switches between like type from type one aggregation to type two aggregation and when it's complete the auto
Gajinder Singh: Yeah.
Anshal Shukla: right
Gajinder Singh: So this is the window thing and the third thing is about uh about uh the
Anshal Shukla: there splitting of signatures. Yes.
Gajinder Singh: yes no third thing is about starting the type one aggregation in the previous
Anshal Shukla: Okay.
Gajinder Singh: slot the previous interval and then the fourth thing would
Anshal Shukla: Oh,
Gajinder Singh: be the metrics for the splitting.
Anshal Shukla: so even if it's even if it it starts like aggregation type aggregation and
Gajinder Singh: Right.
Anshal Shukla: uh the last interval of the slot uh it will it will the blocker might still have to do the type one aggregation again if there are like more uh aggregates that is received from the gossip or there's like a delayed gossip from any of these subet


00:18:45

Gajinder Singh: Yeah. So, but but the thing is that you can you can say that okay,
Anshal Shukla: proposals
Gajinder Singh: that's it. You're not going to do anything more.
Anshal Shukla: okay I'll add that I would like to have like a interval is specific thing where we time out these attestations because I don't want uh it to continue to happen even when it becomes irrelevant after like few seconds or stuff. So yeah, I look into
Gajinder Singh: Yeah. So,
Anshal Shukla: it.
Gajinder Singh: so we basically say that type one aggregation definitely should not exceed beyond the zero interval, right? So we have type zero aggregation starting from previous interval and maybe we say that by the mid of the first of the zero interval then we stop it and then we only do type two aggregation and let it run till
Anshal Shukla: That's
Gajinder Singh: the mid of uh first interval because but but people
Anshal Shukla: Yeah.
Gajinder Singh: might but it could be yes by the end of the first interval people could could still do attestations. They could still delay and do a test stations by that time.


00:19:58

Gajinder Singh: But I don't think nodes do that. As soon as the first interval is checked, they will start doing the test stations. But even then, I think having a delayed block might not be bad.
Anshal Shukla: Yeah, it depends how delayed it it is. If it is like few intervals late then it's fine. If it is few slots away then I think it's not okay because it will just hang up the resources that will be eventually like eventually that shouldn't be an issue because we'll have like a lot of uh proposals and like any proposal uh proposal won't be proposing too many blocks like within a small interval. So it should be fine that way.
Gajinder Singh: So so so there are there are two things right if if you are pack if you are packing if you need to pack something from the site branch and which basically site branch move just justification and finalization forward then that is a block that can't be missed right so even if it's taking more time You will let it basically go out there because nodes will import it and it will be useful work for them that otherwise no no node might be able to pack it


00:21:02

Anshal Shukla: Hey.
Gajinder Singh: up unless split is available but so it depends upon whether the split is available or not. split is the only thing I think which can so this is the only use case only case where I think we can basically let our delayed block in uh production happen but even when this is happening then so if we produced a split I think we should also uh first of all send it to the network to the
Anshal Shukla: Okay.
Gajinder Singh: aggregated network so that Others can have the split if we did the work and we were not able to produce block in
Anshal Shukla: Yeah.
Gajinder Singh: time.
Anshal Shukla: So as soon as we produce a split we just cross it amongst the other among
Kai Chen: Have fun today.
Anshal Shukla: within the network and I think like it's okay like we don't need we don't really need a interval for block production because I think I was thinking more in terms of like the current deet size where there are very less uh proposers as such so we have like frequent turns so that might create a lag and like a bunch of blocks that we want produce that are in the aggregation phase but in like eventually with higher number of validators that won't be the case.


00:22:34

Anshal Shukla: You'll have like good enough gap between two proposals for a particular validator.
Kai Chen: See?
Anshal Shukla: In that case just there won't be like any uh evaluation
Kai Chen: Let's
Anshal Shukla: of
Parthasarathy Ramanujam: But you'll be aggregating a lot
Gajinder Singh: So even now right now there is no starvation of resources that we are seeing right for example on the network that Parthur
Parthasarathy Ramanujam: more.
Gajinder Singh: Ryan it is just that things are delayed because they are taking lotion is delayed.
Parthasarathy Ramanujam: Yeah.
Anshal Shukla: Yeah. So right now starvation can happen because we have like every other slot I have to make a proposal.
Kai Chen: see.
Anshal Shukla: So before I end the proposal of my previous slot I have another proposal in line. But that won't be the case in mainet.
Parthasarathy Ramanujam: No,
Anshal Shukla: So that's what I meant that
Parthasarathy Ramanujam: but so what we
Gajinder Singh: Yeah. Yeah. One one one thing just just one thing we we need to make sure that
Kai Chen: It's
Parthasarathy Ramanujam: are
Gajinder Singh: proposal doing proposal and estation is like a singleton thing, right?


00:23:20

Kai Chen: It's
Gajinder Singh: So no two proposals are being built at the same time. uh so if the previous proposal is being built then we should either discard it or discard the current proposal or whatever whatever strategy I think we need to figure out over there.
Kai Chen: something
Gajinder Singh: So this is one of one of another PR that we need to make sure that
Kai Chen: up.
Gajinder Singh: again proposal and attestations need to be singleton at any given time. So if if you are not in the interval basically I think discarding it is the best way to go for it. First you need to make sure that things are discarded.
Parthasarathy Ramanujam: Okay.
Gajinder Singh: So yes, let's fix hard limits till what these uh these things these computations can run for
Kai Chen: Where's
Gajinder Singh: and I think for block proposal it should be mid of first interval and for a destation it could be the mid of second interval right so and let's start the type one aggregation right away in the previous
Parthasarathy Ramanujam: Okay.
Gajinder Singh: slot last interval of previous slot.


00:24:42

Gajinder Singh: So these these are so can we basically you
Kai Chen: So,
Gajinder Singh: know form the list of PR that we need to do and can we get them out by this weekend
Kai Chen: we have this
Gajinder Singh: itself
Anshal Shukla: Yeah. So, uh I think uh I'll do this aggregation and stuff. Python if you want to take up like matrix and loging stuff you can take it. I think you already have pretty wide suit of additional matrix. So if you want to take it up then you can uh else I I'll create those p as well.
Parthasarathy Ramanujam: So uh if uh because you and Kai have been working on the original spec implementation once you add those stuff if there is any additional iteration or improvement to do that I will do. So the or if the first one if you can put through that would be better.
Anshal Shukla: Okay. Okay. Okay. Okay. I'll do that.
Kai Chen: Thank you.
Anshal Shukla: I think if I already have a PR, so I'll look into that as


00:25:39

Parthasarathy Ramanujam: Sounds
Anshal Shukla: well.
Gajinder Singh: Yeah guys, PR is quite simple.
Parthasarathy Ramanujam: good.
Gajinder Singh: So just add the log file over there and let's merge that log line over there and let's merge that PR
Parthasarathy Ramanujam: Okay.
Gajinder Singh: itself.
Parthasarathy Ramanujam: Uh there is one bug fix PR I have as well. Uh we had that external contributor who implemented a change while testing. I ran into one issue which we have to address. We added a filter to the blocks by route uh which is causing some issues. So if uh you can approve that PR I think I forgot the PR number uh I think it's
Gajinder Singh: I think it's Yeah,
Parthasarathy Ramanujam: 995. Yeah.
Gajinder Singh: I'm looking at
Parthasarathy Ramanujam: Yeah.
Kai Chen: Repeat.
Parthasarathy Ramanujam: Uh that's one and one further update I mean I've been testing Zig Lib P2P
Gajinder Singh: it.
Parthasarathy Ramanujam: implementation with Zim. uh it's working fine with his uh only Z nodes but uh facing few interrupt issues with gossip alone uh with the rust implementation. So I'll be uh doing that separately and once I'm satisfied it's running for over


00:26:36

Kai Chen: Awesome.
Parthasarathy Ramanujam: 24 hours we can get that PR merged as well. I'll set it for review once it's ready.
Gajinder Singh: Got it. Sounds good. And Kai, how is the shadow sims coming along?
Kai Chen: uh yes. for so for shadow shadow branch I already have merged merged the the latest code and uh uh make make the shadow branch uh works and I already test several runs you use the use the uh uh use the shadow So uh father and uh for uh I just run uh 32 nodes uh and uh and it it run about uh each run run about uh 1,000 uh slots. I I saw the result is uh is okay but it it only works uh with uh one one sublight. So when when I when I switch to B supply so I I saw some problem and I think it's it's also the the same problem with the type two merge when the yeah when the uh type one testation is more uh the the the type two merges uh uh uh consumer a lot a lot of time.


00:28:39

Gajinder Singh: But but K on on that then we don't need to log the time
Kai Chen: Yeah.
Gajinder Singh: taken by the type two right we can say that type two time is zero we don't need to log that time on sharing
Kai Chen: Uh yes. So, so I have a config. So I uh preiously I just set uh like that config
Gajinder Singh: I'm
Kai Chen: match the real uh real uh real type to uh so uh I I I already change it to to to a more so so I already change that bra and uh uh increase it. Uh, so I still wait the results right now. It is still
Gajinder Singh: All right, sounds cool.
Kai Chen: running.
Gajinder Singh: uh and let's reconvene on Monday to basically talk about all these issues again because I think we are making good progress and let's keep up the pace and be the front leader in definite 5. So, anything else we have to talk about?
Anshal Shukla: So I have a EIP 892. Uh I think it is ready now.


00:30:12

Anshal Shukla: So I got access to it to create a uh the switching topic there. Uh I just got it today.
Gajinder Singh: Super. I'll take a look at it and see if we can have a draft match.
Parthasarathy Ramanujam: Can you share that uh link and also your problem with ethmizations uh is uh solved
Anshal Shukla: Yeah. Yeah.
Parthasarathy Ramanujam: now.
Anshal Shukla: So, uh I didn't have like the proper role. Uh totally updated my role.
Parthasarathy Ramanujam: Okay. Okay.
Anshal Shukla: I'll put it on the
Gajinder Singh: All right. Then anything on the keys store PR front? EIP key store EIP front.
Parthasarathy Ramanujam: Uh so I had a question there uh Gajender what do we do because uh the original uh EIP that uh I put together was even before they finalized on um uh any of the crypto stuff completely now this is more a key gen so we might have to revisit that EIP completely I'll have to see what is the method used. Maybe what is already being suggested uh might work but uh uh I'll probably work through


00:31:32

Anshal Shukla: I
Parthasarathy Ramanujam: that uh over the weekend and share that draft with you.
Anshal Shukla: foundation.
Parthasarathy Ramanujam: Um the problem is when I was there I had transferred that EIP to the remlabs repository. Um uh I I don't have access to that anymore. So I'll have to set up a new one in our repo and then we can do from there.
Gajinder Singh: Yeah. Yeah. We don't need basically you didn't create a PR anyway against the
Parthasarathy Ramanujam: Yeah, I didn't create a P.
Gajinder Singh: EIPs many IPs.
Parthasarathy Ramanujam: It was just
Gajinder Singh: Yeah. So, so it doesn't matter.
Parthasarathy Ramanujam: Yeah.
Gajinder Singh: You can just you know recreate it and create a PR against the
Anshal Shukla: I We have a lot
Gajinder Singh: EIP directory this time because let's will merge it for draft for the
Parthasarathy Ramanujam: Okay.
Anshal Shukla: of
Gajinder Singh: draft purposes and assign it any
Parthasarathy Ramanujam: Sounds good. Sounds good.
Gajinder Singh: number.
Parthasarathy Ramanujam: Last time uh Justin wanted me to get feedback from uh Antonio Sanso, but uh Antonio never came back to me.


00:32:33

Parthasarathy Ramanujam: Um I mean once we have this again maybe uh I don't know who else should take a look at that uh draft before we proceed because Toma said he
Gajinder Singh: So no we so we can we can have a draft and then then we can do iterations on it.
Parthasarathy Ramanujam: is the right person.
Gajinder Singh: Nothing is stopping us to do that. Right.
Parthasarathy Ramanujam: Okay.
Gajinder Singh: So,
Parthasarathy Ramanujam: All right. Yeah. Yeah. That's fine then.
Gajinder Singh: so
Anshal Shukla: The cryptograph
Gajinder Singh: the
Parthasarathy Ramanujam: Uh,
Anshal Shukla: that it is finalized
Parthasarathy Ramanujam: cryptography for
Anshal Shukla: Yeah.
Gajinder Singh: so even if cryptography is not finalized that means that the this EIP will not be used right there will be some other EIP for the new new cryptography or even this EIP could be
Anshal Shukla: No.
Gajinder Singh: updated. But I think we should just start with it and then basically throw it out on open and then let things roll
Anshal Shukla: Yeah.
Parthasarathy Ramanujam: Yeah.
Gajinder Singh: on.
Anshal Shukla: So like my is I think independent of all the cryptography


00:33:35

Parthasarathy Ramanujam: Okay.
Kai Chen: Is
Anshal Shukla: stuff that is near but I think part of the will be
Kai Chen: that
Anshal Shukla: dependent on cryptography to some extent not much.
Parthasarathy Ramanujam: Yeah, I mean the way Porters responded to one of those questions, it looks like uh it'll take a quite a bit of time convincing the core devs of this passing ECD. I mean not uh I mean the lean thing itself not our EIP in particular but in general
Anshal Shukla: I feel like I think I think we're working on
Kai Chen: That's
Parthasarathy Ramanujam: yes I have a version Gajender had a look at it but uh I mean it's not completely
Kai Chen: it.
Parthasarathy Ramanujam: reviewed uh I can share that link as well But there is a lot of change because Glamsterdam uh EIP list is not uh I mean although it's frozen the implementation is not ready there might be a lot of changes that we might have to do and uh on on that question
Anshal Shukla: Yeah, I think like they have
Parthasarathy Ramanujam: um sorry the the question I had is other teams seem to be uh taking help from


00:34:56

Anshal Shukla: There's a
Parthasarathy Ramanujam: EPF participants to implement the E integration layer.
Anshal Shukla: couple
Parthasarathy Ramanujam: Uh I don't think anybody is contributing to our uh code base for the CPF but uh we I was just wondering is that something we should also consider? Gajender this is more a question for
Anshal Shukla: ingredients.
Parthasarathy Ramanujam: you.
Gajinder Singh: Yeah. So, I guess we can drop in a project over there. It's not really an issue. So if you any of you want to write it in I guess we can do
Parthasarathy Ramanujam: Okay.
Gajinder Singh: it.
Parthasarathy Ramanujam: Okay, sounds good.
Anshal Shukla: Okay.
Gajinder Singh: Cool.
Anshal Shukla: Thanks.
Gajinder Singh: So who's going to do the EPF thing then? Who's going to run with it?
Anshal Shukla: I think I had discussed this with you earlier as well. I'll write that proposal about adding the text fun and overall caching here and there in the CI itself to improve the CI. But if wants to write about he can I
Gajinder Singh: So so so basically this is our devet 6 proposal right.


00:36:11

Gajinder Singh: So so we can just frame it like
Anshal Shukla: Yes.
Gajinder Singh: that.
Parthasarathy Ramanujam: Yeah, we need yeah basically engine API integration uh within Zim.
Anshal Shukla: Let's
Parthasarathy Ramanujam: Uh that's the high level requirement which is what I believe re and grand are doing as
Anshal Shukla: see.
Gajinder Singh: Yes
Parthasarathy Ramanujam: well. Um but yeah we can narrow down the
Gajinder Singh: to tell you basically engineer
Parthasarathy Ramanujam: scope.
Gajinder Singh: to I think there is ego from side but
Kai Chen: We're
Parthasarathy Ramanujam: Sorry.
Gajinder Singh: to tell you I think engine API is very easy to implement.
Kai Chen: looking
Gajinder Singh: So I mean I shouldn't I should I don't think that there should any be an issue with it or people should just be able to independently do it. So I guess that is okay for us to give it to
Parthasarathy Ramanujam: Okay.
Gajinder Singh: PF.
Parthasarathy Ramanujam: All
Gajinder Singh: It should be a pretty easy integration in my opinion but yes depends
Parthasarathy Ramanujam: right.
Gajinder Singh: upon how much contacts a person has.
Parthasarathy Ramanujam: Yeah. Yeah, for sure.
Gajinder Singh: All right. Uh, do we have anything else for the call? All right, guys. Then we will I'll schedule a call on Monday and let's talk then and take a review on the things that we discussed today. And uh the things that we discussed today, some of those things are super easy. Let's get them down.
Parthasarathy Ramanujam: Yep. Thank you.
Gajinder Singh: Well, guys,
Parthasarathy Ramanujam: Speak to you on Monday.
Gajinder Singh: thank you. Yeah.
Parthasarathy Ramanujam: Thank you.
Anshal Shukla: Thank you.
Parthasarathy Ramanujam: Thank
Anshal Shukla: Bye-bye.
Gajinder Singh: Bye-bye.


Transcription ended after 00:38:10

This editable transcript was computer generated and might contain errors. People can also change the text after it was created.
