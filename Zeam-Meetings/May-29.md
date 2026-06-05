May 29, 2026
Zeam call  - Transcript
00:00:00
 
Gajinder Singh: Hello everyone, welcome to May 29 Zoom call. Uh the main aim or and the emphasis of the call is again to resolve DevNet 4 issues on a scaled u network with multiple subnets and a high count of valid data relatively high count from what we are used to testing. So let's go directly let's directly jump into that particular issue or or before that maybe we can talk about uh the DevNet 5 and shadow stuff with Kai. So Kai uh let's start with your update.
Kai Chen: Uh yes. uh this this week mostly working on the uh develop five uh uh implementation because uh uh by the end of last week uh once I finished the development I found the uh uh accounts uh account work by with uh three nodes uh that night. So I do uh much uh investigation and find some bugs. Uh so uh right now uh I can I can I can run the three nodes uh link star. Yeah. uh net uh five works uh and uh and I also do uh use the interop with uh this lambda net 5 image.
 
 
00:02:06
 
Kai Chen: Uh it it looks like uh works. uh but uh but there there are some uh issues remain uh so I uh from my observation uh the the resource uh consume uh by Islam is is better than us. Uh it it only use uh five about 500 uh mgabyte memory but our node uh uh always uh consume about two gigabytes. memories. So when when the network uh happen some uh not stability when it's chemical chemical some uh fork forks so our uh our no uh catch up is not quite uh st stable. So uh I still need uh figure out why we use so much uh memory uh and uh and another issue is about uh uh steam cast sim test. So I think the the current uh GitHub CR resource cannot cannot run the DET file uh three nodes sim test. Yeah. So right now I I do some work uh in sim test. It it it does not do real uh proof uh compaction. Uh so I also saw if right now we have a hive test.
 
 
00:04:45
 
Kai Chen: So maybe we we can we can disable the sim test.
Gajinder Singh: So I don't want to disable sim test but in the sim test can be configured to run on bigger slot times so that this problem is resolved in devet 5 as of now but then when we fix all of these things then we can uh basically reduce our slot time. So from 4 second can we just bump our slot time to 8 second? If that is also if not then to 12 second in the same
Kai Chen: Um,
Gajinder Singh: test.
Kai Chen: so right now uh I can I I can successfully uh run the sim test in my local. So it's it's the resource uh limit in
Gajinder Singh: Yeah. Yeah. So can so for the CI can we configure it to run with 12 seconds slot time because
Kai Chen: in
Gajinder Singh: I think interval plus plus slot will automatically will calculate it right because we have that calculation in the params. So, so in in CI can we basically by an environment variable or through uh I mean let's figure out uh
 
 
00:06:05
 
Kai Chen: Okay. Okay.
Gajinder Singh: I think you can configure the DevNet spec in the in the sim
Kai Chen: Okay.
Gajinder Singh: test in the beam command right so if if you can increase the slot time over there and if it's the slot time is not picked over from over there maybe we we do the change so the slot time can be picked from over there and uh make sure that we are able to run in the
Kai Chen: Uh,
Gajinder Singh: CI.
Kai Chen: okay. I
Gajinder Singh: So with this change let's get the CI working also with each lambda if we are interrupting do
Kai Chen: can't
Gajinder Singh: you have some screenshot where we are finalizing so that I can post an inter interrupt successful milestone on our zoom channel Twitter
Kai Chen: Yes. Yes.
Gajinder Singh: channel.
Kai Chen: I can after this meeting I can I can do that
Gajinder Singh: Awesome. And with regard to memory,
Kai Chen: understand.
Gajinder Singh: if our if your machine has sufficient memory, then we we and E lambda keeps keep on finalizing and working on right.
 
 
00:07:12
 
Gajinder Singh: So the only issue is when you are low on memory, right?
Kai Chen: Yeah. Yeah. Yeah. But I I don't So I don't run uh a a long
Gajinder Singh: So,
Kai Chen: time test. So maybe uh I I can
Gajinder Singh: so we we don't need a long time run basically just test that we are
Kai Chen: run.
Gajinder Singh: continuously uh finalizing
Kai Chen: Yes. Yes.
Gajinder Singh: maybe.
Kai Chen: Yes. It it already uh it's already works.
Gajinder Singh: Yeah.
Kai Chen: It can
Gajinder Singh: All right. So for a high finalization uh screenshot just
Kai Chen: falize.
Gajinder Singh: share with us on the Zam channel and then I'll create a post on on the Twitter using that uh so that we declare that yes uh we have achieved this particular milestone with e lambda and uh and once we once uh we do that the next thing to do is basically do shadow runs for devet
Kai Chen: Yeah. Yes. Yes. Um, yes, I'm working on that uh right now.
 
 
00:08:19
 
Kai Chen: Yes.
Gajinder Singh: Okay.
Kai Chen: Uh,
Gajinder Singh: So let's let's get this PR ready uh with uh that particular change in the spec where we can run the run NCI at a higher time uh at higher slot inter uh slot time and then give the give me the interop uh screenshot that you have with the lambda and then let's start and let's get the shadow branch ready right so let's move the shadow branch from devet 5 devet port to Devet 5 and then basically give in the shadow channel of the PQ interrupt. Let's update that our DevNet 5 shadow branch is ready for sim test and uh uh Kamil can basically start doing it as well as let's procure a server and let's start running sim test shadow sim test ourselves as well.
Kai Chen: Okay. Okay.
Gajinder Singh: Okay, cool. All right. Uh any other DevNet 5 discussion we have
Anshal Shukla: No,
Gajinder Singh: an
Anshal Shukla: so the only thing is that uh today I I think I joined late so I don't know if it has been covered but ML today updated is aggregation branch so that has improved the aggregation times.
 
 
00:09:42
 
Anshal Shukla: So did we update it on the uh DevNet 5 branch as well? K.
Kai Chen: So is it emerging
Anshal Shukla: Yeah, you can uh look at the message in the DevNet 4 channel.
Gajinder Singh: So, Angel, can you just give Kai the message link?
Anshal Shukla: Yeah.
Gajinder Singh: That would be
Anshal Shukla: Yeah. Yeah, sure.
Gajinder Singh: easier.
Kai Chen: Oh, I I I don't I don't I don't try that. So, because I uh I just focus the testing with current my implementation. So yes, I think when I finish the the the requirement from Genda. Yeah, I can try
Gajinder Singh: So K I think the first thing that you can do is just update the thing and then let's do everything else
Kai Chen: it.
Gajinder Singh: because let's run with the latest
Anshal Shukla: Yeah.
Gajinder Singh: uh
Anshal Shukla: So I was saying that it might help us get a finalization and CI as
Gajinder Singh: Yeah. Yeah.
Anshal Shukla: well.
Parthasarathy Ramanujam: But it's just 15% improvement I think not uh major All
Kai Chen: Uh so I
 
 
00:11:07
 
Anshal Shukla: Yeah. So it's
Kai Chen: so I used I used the I used the commit uh which you uh you
Parthasarathy Ramanujam: right.
Kai Chen: used and so is it comp compatible with that change as I saw ML send it in the DT four. Yeah.
Anshal Shukla: Yeah,
Kai Chen: So, I'm not
Anshal Shukla: he send it on four but he mentioned that he has merged it with devet 5 as well.
Kai Chen: sure.
Anshal Shukla: I think so he mentioned yeah he did mention devet 5 has been updated with the latest changes from main the latest commitment is a def c
Parthasarathy Ramanujam: Yeah, but it's a breaking change, right? So, there may be some change required from our glue as well. I don't
Anshal Shukla: Yeah, it's a breaking change because right they have they now return a error instead of
Parthasarathy Ramanujam: know.
Anshal Shukla: panicking. So it isn't like a huge change. It's just like the API has changed to return an error as
Parthasarathy Ramanujam: Okay.
Anshal Shukla: well.
Parthasarathy Ramanujam: And the ZK alloc require a separate build configuration maybe.
 
 
00:12:08
 
Anshal Shukla: Y but that is also that is something which is optional.
Parthasarathy Ramanujam: Um
Kai Chen: Thank
Parthasarathy Ramanujam: uh I mean if we are making the change ideally we should do that as well because it'll probably reduce
Kai Chen: you.
Parthasarathy Ramanujam: the I mean improve the performance. He says it's 15% more performant if we use ZK lock but
Anshal Shukla: Yeah, but also that it might corrupt the memory.
Parthasarathy Ramanujam: uh yeah it may corrupt. Yeah. So I don't know maybe to start off we just uh use the latest version.
Anshal Shukla: So
Parthasarathy Ramanujam: Oh Kai has dropped off then
Anshal Shukla: yeah, I'll ping him this.
Parthasarathy Ramanujam: probably.
Anshal Shukla: But I think we can definitely use it on the CI at least to like
Parthasarathy Ramanujam: Okay.
Anshal Shukla: get the CI to pass.
Gajinder Singh: Sure.
Parthasarathy Ramanujam: Okay,
Anshal Shukla: But later on we can decide upon like the other things if you want to use it on the main or not.
Parthasarathy Ramanujam: sure.
Gajinder Singh: All right. Sounds like a plan. U so basically coordinate with Kai on this and let's sort of check mark the DENet 5 milestone and start shadow runs on it.
 
 
00:13:16
 
Gajinder Singh: Now moving on to DevNet 4. Yes papa. So,
Parthasarathy Ramanujam: So uh several runs and several new issues uh we came across uh the major uh problem with Ze um aggregator I think we have mostly solved but uh I don't know what has happened one of the clients has uh released a new image For the past two days, we've been running into issues with the snappy frame being corrupt. Initially, I thought it was a problem with Ze uh implementation because others
Gajinder Singh: It it won't be it won't be a snappy frame just to interject.
Parthasarathy Ramanujam: were Yeah.
Gajinder Singh: It is just snappy
Parthasarathy Ramanujam: Yeah. Sorry, not snappy frame, snappy issue. Uh snappy corruption is what was being reported.
Gajinder Singh: issue.
Parthasarathy Ramanujam: I thought it was a problem with Z implementation but it turns out after several tests that uh all of these happen when ETH lambda is the proposer. uh the problem it presented on Zim was that in case of such a a corruption failure, Ze didn't have the recovery logic written correctly. Um so there was a bug in our uh call to blocks by root which never got triggered.
 
 
00:14:32
 
Parthasarathy Ramanujam: So Zam client stalled others were reporting the same error but they were progressing uh they rejected that particular uh uh gossip and started to proceed but we didn't. So,
Gajinder Singh: But but why why but why would we need block by root? So first of all I think we should change the name of the error because there is no corruption that has happened.
Parthasarathy Ramanujam: um
Gajinder Singh: We will we can just rename it saying you know bad snappy bad object or you know whatever
Parthasarathy Ramanujam: yeah. Yeah. The Yeah.
Gajinder Singh: encoding because because that
Parthasarathy Ramanujam: Bad decode of that sort. Yeah.
Gajinder Singh: error sort of corruption says that our memory is corrupted and it basically have some other conotations that other people will make from it. So let's I think first of all change the name of the error in the sense that snappy input incorrect or invalid or whatever right. Uh so let's first do that and then why do we need blocks by root
Parthasarathy Ramanujam: Okay.
Gajinder Singh: because if that block is bad we just keep it on the side and the next we should get a next gossip block right there is no block by
 
 
00:15:38
 
Parthasarathy Ramanujam: Correct. But the problem is this. Yeah, this this has happened when it at slot one the very first slot uh which was reported to
Gajinder Singh: root
Parthasarathy Ramanujam: us uh we rejected it. We didn't know what our current state was but others had already proceeded. So ideally um Zen should have caught up the current
Gajinder Singh: So what do you mean? Do you mean others have currently proceeded?
Parthasarathy Ramanujam: state
Gajinder Singh: If we rejected, everyone should have rejected it, right?
Parthasarathy Ramanujam: correct but everyone had rejected it.
Gajinder Singh: So that that apart from ETH lambda, nobody has that block.
Parthasarathy Ramanujam: Yeah, eat lambda was the aggregator of that block.
Gajinder Singh: I mean proposer. You mean proposer?
Parthasarathy Ramanujam: Oh yeah, proposer. Sorry. Uh so eat lambda was the proposer which generated
Anshal Shukla: It's
Gajinder Singh: So,
Parthasarathy Ramanujam: that.
Gajinder Singh: so most likely ETH lambda added it to its chain but none none of the other codes are
Anshal Shukla: human.
Parthasarathy Ramanujam: Yeah,
Gajinder Singh: right.
Parthasarathy Ramanujam: correct.
 
 
00:16:30
 
Gajinder Singh: So why should they do block by root? There is no block by root to be done. So next block anyone should propose if it's not eth lambda should be on top of genesis
Anshal Shukla: Also
Gajinder Singh: itself.
Parthasarathy Ramanujam: Right. So,
Anshal Shukla: not on slot one if I remember correctly like we were seeing it on Wednesday
Parthasarathy Ramanujam: uh
Anshal Shukla: as well and it happened like way past after finalization beyond
Parthasarathy Ramanujam: that so I' I've had several runs. The issues that we saw yesterday, day before yesterday were on slot 4951. this morning it's been on slot one. So I'm not sure uh where it is but whenever something like this happens uh blocks by root was not being triggered as a result uh zing did not proceed at all.
Gajinder Singh: But block block by root should not be triggered.
Parthasarathy Ramanujam: installed not
Gajinder Singh: Why? Why should block by root be
Parthasarathy Ramanujam: because of this but uh because there is nothing coming through uh Zen
Gajinder Singh: triggered?
Parthasarathy Ramanujam: assumes that it is behind the chain.
 
 
00:17:29
 
Parthasarathy Ramanujam: So it requests uh other peers to send through uh what is the current uh state right in order to catch up
Gajinder Singh: No.
Parthasarathy Ramanujam: that
Gajinder Singh: So if for example if it's happening on the slot one itself then there is nothing to catch up on.
Parthasarathy Ramanujam: not on slot one.
Gajinder Singh: So what so did Zim and others continue
Parthasarathy Ramanujam: Yeah.
Gajinder Singh: to do block production and sort of progress
Parthasarathy Ramanujam: Okay.
Gajinder Singh: there?
Parthasarathy Ramanujam: So, uh let me say two things. Slot one happened just now while we are on the call. I just posted that message on the PQ interrog group yesterday and earlier this morning. It was much further down slot 49, slot 51. every time it's at a different slot but it definitely happens at some slot where there is a problem. I have updated all of this in issue 942 each observation that has happened uh after each restart. Um so the current
Anshal Shukla: I just thing to confirm if like in Zim we do
Parthasarathy Ramanujam: fix
Anshal Shukla: like a syncing if there's a finalization that we haven't seen and then we should call like block by root
 
 
00:18:40
 
Gajinder Singh: So, so when Zin install we we are we are we not seeing any gossip messages?
Parthasarathy Ramanujam: Yep. No gossip messages,
Gajinder Singh: So, go is gossip messages not coming.
Parthasarathy Ramanujam: nothing.
Gajinder Singh: We are still subscribed to LP2B right
Parthasarathy Ramanujam: Yes. So this is again my hypothesis that other clients have started to reject us as a peer because
Gajinder Singh: topics.
Parthasarathy Ramanujam: we have not uh drained the queue quickly enough. That that was my hypothesis.
Gajinder Singh: So, so is our connection number of connection count going down?
Parthasarathy Ramanujam: No, it hasn't. Uh we still I think in peer number of peers connected we still have 59 peers that we are connected to but the active uh I don't know um the mesh might be
Gajinder Singh: So did were you were you able to I mean it doesn't matter what the mesh is we
Parthasarathy Ramanujam: different.
Gajinder Singh: should get all the gossip messages. Uh so I am not able to SSH to one second. Okay, I'm able to SSH do lookup PS. Let me just look at current logs on Zmate.
 
 
00:20:05
 
Gajinder Singh: So, so if where are we on this? Okay, we are stalled again on I think 271 minus 173 or whatever that is which could be again like 51 kind of a slot number.
Parthasarathy Ramanujam: Yeah.
Gajinder Singh: So again yes it's what is the relevance of this particular slot number I will I will look at the log and
Parthasarathy Ramanujam: So, uh it's not deterministic in terms of slot number.
Gajinder Singh: see first first of
Parthasarathy Ramanujam: something is going on in the snappy uh encoding
Gajinder Singh: all I why has not debug not been switched on I mean this is my question
Parthasarathy Ramanujam: of I I did enable debug but it I I did enable
Gajinder Singh: again but I I only I am only seeing info logs I'm not seeing debug
Parthasarathy Ramanujam: debug
Gajinder Singh: logs at all if I do local logs and I I'm not seeing
Parthasarathy Ramanujam: uh
Gajinder Singh: any debug log all all of them are info log and that's why I can't debug and we have an issue with
Parthasarathy Ramanujam: one
Gajinder Singh: logs.
Parthasarathy Ramanujam: there. Uh you should be able to see it in consensus.log, right?
 
 
00:21:11
 
Parthasarathy Ramanujam: That that is also not available. I don't know what's wrong with
Gajinder Singh: But did we fix our file writing thing because we had
Parthasarathy Ramanujam: uh
Anshal Shukla: We did.
Gajinder Singh: an issue over there.
Anshal Shukla: Yeah. Yeah, we did. Uh I had reviewed one PR from uh Chetan.
Gajinder Singh: Okay. So where where is that particular files in zam folder?
Parthasarathy Ramanujam: Uh it should be under sorry I don't know where exactly this is. Uh it should be under leanquick start uh consensus.log
Gajinder Singh: So I mean I am on this particular server. Okay. I in zoom we have lean quick start. There is nothing over
Anshal Shukla: What's the IP?
Gajinder Singh: there.
Anshal Shukla: Can you tell me like the initial digits
Gajinder Singh: What initial digits?
Anshal Shukla: of the IP?
Gajinder Singh: Okay. I think 77.42.121.211.
Parthasarathy Ramanujam: Oh, that's the aggregator. Yeah.
Gajinder Singh: Yes. Okay. Let's resolve this log issue. PA please because how are we even about that and
 
 
00:22:50
 
Parthasarathy Ramanujam: Yeah. I So
Gajinder Singh: then I I really see the log and then see where what happens when that thing
Parthasarathy Ramanujam: I
Gajinder Singh: happened because because gossip should not stop coming I mean unless we unsubscribe to it right so where are the gossip triggering logs then basically we can see that what is happening over there and uh Again, if if someone proposes a bad block, it others everyone is rejecting it. Everyone else is rejecting it. So, they should propose a block on top of whatever the parent was. uh and uh then there shouldn't be any block by root request fetching needed because any case the block has been rejected and if ETH lambda must then also reject the block because on the voting it should be reorged out right most of the clients would be voting it out so it should be regged out by it lambda as well so that block should be reoed
Parthasarathy Ramanujam: Yeah. So, yeah, I've just sent you the location of the log file, uh, the debug, uh, log. just uh yeah consensus.
 
 
00:24:14
 
Parthasarathy Ramanujam: That's the location. So the PR that I have 948 addresses some of the issues with that snappy failure and everything else. And uh uh that keeps the image I have built from that PR uh is the one that's running on DevNet right now. And um uh the container doesn't crash after that or it doesn't get stalled. But we still have some other issues such as uh the aggregation signature failure and which is again observed even on Grand is also uh rejecting the aggregate uh that eat lambda has sent through.
Gajinder Singh: So, so okay I mean that is fine but it should not stall us that is my
Parthasarathy Ramanujam: Yeah.
Gajinder Singh: problem.
Parthasarathy Ramanujam: Yeah. Yeah. I I follow and that's what I'm investigating why there's of there's a stall because this behavior is unique to Zim only. Other clients are able to proceed. We are not. And so um I want I'm digging deeper into it to understand what is causing this and if there's any uh discrepancy there.
Gajinder Singh: Because on slot 32 so basically slot by slot we see what is happening right so on slot 32 things to be fine.
 
 
00:26:21
 
Parthasarathy Ramanujam: Yeah,
Gajinder Singh: Uh let's go to slot
Parthasarathy Ramanujam: but if you look at if you look at the latest ones,
Gajinder Singh: 42.
Parthasarathy Ramanujam: we are not receiving any message at all on Z8 at least. There's been nothing on
Gajinder Singh: Yeah.
Parthasarathy Ramanujam: Gossip.
Gajinder Singh: Yes. Because debug logs are not even there. So debug log should help you debug it. Right.
Parthasarathy Ramanujam: No debug logs are there in the file I just sent you,
Gajinder Singh: Now in the file.
Parthasarathy Ramanujam: right?
Gajinder Singh: Now in the file.
Parthasarathy Ramanujam: Yeah.
Gajinder Singh: Yes. Okay. So let's start looking into it. Uh this should not be happening. Uh we should not be stalling and that basically ID should be reed out. We skip a test station. I mean after at slot 42 we are already saying we are behind
Parthasarathy Ramanujam: Yeah.
Gajinder Singh: peers because at slot is 26 and finalized slot is zero and max peer finalized slot is zero. So we are already set so at
 
 
00:27:43
 
Parthasarathy Ramanujam: So that function yeah so that function that determines what the
Gajinder Singh: slot
Parthasarathy Ramanujam: current slot of a pier is was returning null instead of the actual one. So the block spy root never got triggered and uh uh that's why we
Gajinder Singh: blocks blocks by root should not be getting triggered because if you are receiving everything you should
Parthasarathy Ramanujam: were
Gajinder Singh: have the chain right why should blocks that is what I'm
Parthasarathy Ramanujam: we are not receiving
Gajinder Singh: saying you we in the at slot 42 we are receiving all the gossips
Parthasarathy Ramanujam: right.
Gajinder Singh: but why what are we not receiving so we need to basically see why we are not receiving things So who produced slot 42 for example we need to we need to basically go slot by slot and see what is happening right so you need to grab each of the logs and slot by
Parthasarathy Ramanujam: Yeah.
Gajinder Singh: slot and see uh what is the issue for it so I'll start looking into it as well uh all z nodes have you tried that
Parthasarathy Ramanujam: Yeah,
Gajinder Singh: network.
 
 
00:28:44
 
Parthasarathy Ramanujam: the current one is all Z nodes, but I'll probably have to run a separate test net with just all Z nodes locally, but uh
Gajinder Singh: Not this is if current one is all zim nodes then why is this
Parthasarathy Ramanujam: the uh
Gajinder Singh: stalling?
Parthasarathy Ramanujam: no uh the subnet is all Z network. Uh I probably have to run everything here.
Gajinder Singh: No. No. I want all zoom nodes and see how far we are able to go.
Parthasarathy Ramanujam: Okay, I'll I'll do that next and then uh we'll see.
Gajinder Singh: What is our test station performance as of now?
Parthasarathy Ramanujam: Uh we are around 1.2 as I
Gajinder Singh: Can can you remove it lambda and run the network as well?
Parthasarathy Ramanujam: said.
Gajinder Singh: I mean if we know that it lambda is a is a issue let's remove it for
Parthasarathy Ramanujam: Yeah. Yeah.
Gajinder Singh: now while we
Parthasarathy Ramanujam: Let me do that. Yeah.
Gajinder Singh: are
Parthasarathy Ramanujam: The thing is I want them to first copy the logs over before uh I restart. I I've pinged them.
 
 
00:29:43
 
Parthasarathy Ramanujam: Uh once they've copied whatever they want, I'll
Gajinder Singh: just just copy it into some back folder on the same server right they can jump and
Parthasarathy Ramanujam: Yeah,
Gajinder Singh: look look look at it I mean sort of make these smart decisions you don't have to wait for
Parthasarathy Ramanujam: sounds good.
Gajinder Singh: them. What about what what else we need to do on the aggregator performance?
Parthasarathy Ramanujam: Yeah.
Gajinder Singh: Are we able to get our priority thing
Parthasarathy Ramanujam: So the go
Anshal Shukla: So I have added yeah I have add added that uh
Gajinder Singh: done?
Parthasarathy Ramanujam: on
Anshal Shukla: on my branch and I uh was planning to test it after this call. I have got a note from Bara. So I was uh hoping to do it after this call and uh apart from that like I looked into Lin as well and there I can see that they have like a strict one aggregation policy right now and they just do like a naive aggregation uh as an aggregator. They just see like whatever is the first signature they have it in the map.
 
 
00:30:48
 
Anshal Shukla: It uh they basically uh basically go over through the every other uh signatures of the uh same subnet and uh aggregate the payloads and that's all they do in as an aggregator. They don't do like a parallel aggregation across multiple attestation data. They just do it once and uh return the data. So uh instead of doing exactly that I was uh I am uh I'll be doing like a uh like I'll be just doing as per like our latest uh justified. So whichever will help move the latest justified. So uh whichever attestation data has source as our latest justified I'm I'll be aggregating that if that is not present then uh I'll be looking into like other uh things. I have that code ready. I'll be testing it out and then I'll share that as well.
Parthasarathy Ramanujam: Yeah. Uh Anel, I believe uh Toma from ETL lambda said there was a bug in the um I mean justification logic of uh the spec. I'm not sure if you had a look at it that was merged to lean spec I think last week.
 
 
00:31:59
 
Parthasarathy Ramanujam: Uh I'm not sure whether Ze is updated with that.
Anshal Shukla: So the main branch hasn't been updated to that but I have looked into that. I'll have added that thing in my uh branch that I working
Parthasarathy Ramanujam: Okay.
Gajinder Singh: Okay. Uh all right. plan. Put the plan on
Parthasarathy Ramanujam: Um, I mean, I would first probably resolve this.
Gajinder Singh: this
Parthasarathy Ramanujam: uh uh the snappy issue uh once that is merged uh we can start uh iterating over Anel's performance improvement for aggregation try to bring it down uh hopefully this 948 PR should bring some improvement to uh Ze's robustness after that we can start working on
Gajinder Singh: Okay, with regard to your SSDPR uh unshell for cloning, it would be using size of size recursive sizing right of the things.
Anshal Shukla: So it it is I have already raised the PR it isn't using rec so it is recursively going over through the uh through the data structure and allocating on the go. So it doesn't use like size of uh preemptively instead it does like recursive iteration through the fields of the strct and then does the allocation because uh there can be like there can be a vector inside a strct and in that case there isn't any max size that is pre-nown I have already raised a PR uh so it does like a recursive thing but it doesn't preemptively allocate it does it on the fly.
 
 
00:34:05
 
Gajinder Singh: Because there is no list in it. That is why
Anshal Shukla: Yeah. So, lists can be have can have like variable
Gajinder Singh: Right? So for if for example you have to clone a list,
Anshal Shukla: sizes.
Gajinder Singh: you will have to recursively figure out the list element size because it could be variable as well. Right?
Anshal Shukla: Yeah.
Gajinder Singh: So but I also don't see any list switch for list handling or there is list is like a strruct. So is there have you checked that it's a list or not?
Anshal Shukla: Yeah. So list is like a struct. So that's why there isn't an explicit list as such. But it I think I have added a test case to handle that. I'll just confirm if that if the test case is there or not. But I think it will handle it. If if the test case is not there, I'll add it and uh bring you again.
Gajinder Singh: Because if there is a list then you would need to call recursively sizing.
 
 
00:35:00
 
Gajinder Singh: That is that is for sure is a
Anshal Shukla: Yeah.
Gajinder Singh: very vable list variable type size and And uh were you able to address uh other commands in the other PRs? Caching hash root, hash root because we not only need to cache hry root, we can we also need to cache max size, min size as well as the size of the variable type, the fixed size, right? So these things also need to
Anshal Shukla: So max size min size is not needed because uh max size min size
Gajinder Singh: c
Anshal Shukla: is not needed because uh the types are comp type so they will already do it while compilation. So uh we don't really need that uh but uh because we pass in these types directly and types are taken in as com time. So whatever places we are basically passing these types it should already be cached in the binary itself. Uh I have looked into like the comments that Gam had raised regarding uh regarding like caching of Merkel uh Merkel leaves. Uh I have a version locally.
 
 
00:36:29
 
Anshal Shukla: The only reason I didn't push it because para in uh sorry Gam in his comment had mentioned that we should like implement a sort of interface there and implement this function on uh for that. Uh I haven't really used interfaces so I plan to look into that how we can do it in Zeg but that's why I didn't push it but I I did address his comment. So, uh, I I I'll try to push it up, but
Gajinder Singh: so if I look currently see the ice fix type is not com
Anshal Shukla: yeah.
Gajinder Singh: time.
Anshal Shukla: Okay.
Gajinder Singh: So at some places it's com time at some places it's not.
Anshal Shukla: Okay.
Gajinder Singh: So can you also make sure that at all places comp time or basically I mean whatever it needs or if it already has a type then it is comp time I don't think so
Anshal Shukla: Yeah.
Gajinder Singh: right we need to change t to comp time t right then it will automatically become comp
Anshal Shukla: Yeah.
Gajinder Singh: time so currently that is not the case then
 
 
00:37:34
 
Anshal Shukla: Yeah.
Gajinder Singh: serialized size will definitely not be comp time and I think you will you will need Uh so serialized so you you definitely can't get serialized size you will need to have min size and max size that could be comp time so we why don't we need min size and max size we should need it for the validation of the object right how how are we sort of doing the validation of that we have the output or why serializing and deserial des serializing we should be needing it somehow. So can you basically also look into this holistically and see if everything is
Anshal Shukla: Yeah. Yeah, I'll I'll do that.
Gajinder Singh: covered or uh uh basically as I talked to GM and I was like okay you know we might need caching in the short run. So basically even with your current approach GM has added some comments were you able or can you address them in the current current approach itself or does it need that uh interfacing
Anshal Shukla: So it doesn't need an interfacing mechanism.
 
 
00:39:07
 
Gajinder Singh: mechanism
Anshal Shukla: I think interface is good to have right now. But uh something like uh using which we can have like uh two hash functions which will one of them will be like uh which will support caching and other one won't be supporting uh the uh supporting the caching mechanism but uh uh I am I have like these individual functions uh I have already pushed that uh so it was there in the original code itself. But I think uh we can define a interface sort of where we can have a have a common common uh function hash function on both the both like we can implement different type of a struct which can implement these two with and without hashing but uh with and without caching but uh I I I'll I'll update it like uh likely this weekend like
Gajinder Singh: Also his question is is cache bringing in performance? Uh most of the types you want are composite types but it should anyway bring performance right because you're not recomputing.
Anshal Shukla: So I I didn't add it for uh composite types because I thought like those will be pretty easy to uh compute.
 
 
00:40:34
 
Anshal Shukla: So will we even have that additional benefit of caching there? But em says that there's a lot left on the table if we are not doing that. So I'll probably benchmark a few things and if because
Gajinder Singh: Yeah, hashing is hashing is an involving process. So definitely it will be especially with regard to if this is happening
Anshal Shukla: accessing
Gajinder Singh: on uh on approver right so hashing is very costly.
Anshal Shukla: okay
Gajinder Singh: So memory lookup is easy compared to hashing. So can yes can we just sort of optimize on all ends and make sure that we aren't leaving any performance on the table with regard to
Anshal Shukla: Yeah.
Gajinder Singh: SSD cloning that will solve it your clone one will solve it
Anshal Shukla: Yeah.
Gajinder Singh: but in that regard just make even uh the clone can go right right now just you need to check whether the variable cloning is working or variable list
Anshal Shukla: Yes.
Gajinder Singh: variable type list cloning is working or not and uh if that is working we can
Anshal Shukla: Oh,
 
 
00:41:51
 
Gajinder Singh: merge in otherwise just make whatever changes are required over there and then what other block what other slower points we have we we used SS clone used to take a long time what else can we think which is a lowhanging group.
Anshal Shukla: so we were making snapshots uh which was also taking a lot of time which isn't required as such because uh these aggregates and payloads that we received are cannot be manipulated uh on the fly as such. So if there's a new aggregate that is formed so it will have like a separate key because of the difference in uh attestation bits. So ideally if there's a payload against a particular attestation bit and uh attestation bits and attestation data then it should uh it won't be changing. So we don't need to do like a snapshotting thing there.
Gajinder Singh: So have We need
Anshal Shukla: That is one thing. Uh we haven't done that yet. Uh so uh I'll be I'll be doing those things.
Gajinder Singh: that.
Anshal Shukla: I have them in mind. Uh I was trying to do like optimize on the uh aggregator side right now.
 
 
00:43:06
 
Anshal Shukla: So uh on the aggregator side I have done it but I haven't done on the block production side. Another thing that uh I wanted to do is that right now I do like a block building stuff which has like the which can have as much as maxistation data but uh and I try to compact all of them and if they fail then I'll be I'll I miss the blog proposal slot. uh instead of that I should only publish the blog which with as many uh attestation data that I have um I have like compacted using.
Gajinder Singh: Yeah. Yeah.
Anshal Shukla: So yeah that is
Gajinder Singh: So, so both in a test uh station building sorry aggregate building and in the block
Anshal Shukla: another
Gajinder Singh: proposal we need to run a timer which basically should exit out at least when we have at least one uh or basically should totally exit out if we have crossed uh you know whatever time we put over there because because then we basically need to leave it and Move on
Anshal Shukla: Yeah.
 
 
00:44:13
 
Gajinder Singh: because that is no more useful
Anshal Shukla: So I was thinking of just uh doing like a single aggregation on the as an
Gajinder Singh: anyway.
Anshal Shukla: uh aggregator because I thought that uh as per my current folk choice rule whatever best statistication data I can aggregate I'll aggregate that and throw it out. Uh and since like I'll be aggregating every slot, I think it just makes sense to follow like whatever is closest to the current justified and once we achieve that then we can like extend it to aggregating multiple attestation data. But even if we aggregate just a single attestation data, it should uh ideally help us move forward with uh finalization. So,
Gajinder Singh: Yeah. So prioritize it, right? So definitely we need to prioritize and say that this is the best adation
Anshal Shukla: uh
Gajinder Singh: data that we'll be picking and aggregating first and then everything if we have the time.
Anshal Shukla: yeah. Yeah. So that's it like from my end like there was few things here and there which I did on the benchmarking stuff and uh I also review reviewed like KA's PR.
 
 
00:45:36
 
Anshal Shukla: I'll sync with Kai regarding like those changes that we were discussing earlier. Uh but yeah that's about it
Gajinder Singh: So when until when can we have this all these things because to be honest
Anshal Shukla: short.
Gajinder Singh: we don't have so much time right we need to be optimized fast and we are sort of banking on you over There heat. So best so for the clone best thing is to not just do it at uh so just take a block of devet 4 which has variable uh a destration data is not variable right and then we have variable signatures right so try to try to do that I think that will basically cover most of cases because we don't only want to check on one level we also want to check whether in the embedded level we are able to do it correctly because as I remember there was few issues which were in that particular area as well because uh when the types the variable types are embedded in the strct then also So we we have not sufficiently covered those if then cases to make sure that everything is covered.
 
 
00:47:44
 
Gajinder Singh: So so take take such kind of an object and maybe start from a simpler normal list as well. So add add a variable type list and then add an object that has that that is a strct which has a variable type list property. I think that should cover it. All right.
Parthasarathy Ramanujam: any server from me.
Gajinder Singh: Coming.
Parthasarathy Ramanujam: Don't say that.
Anshal Shukla: Oh, okay. Okay.
Gajinder Singh: Okay. Coming to you. What is the plan?
Parthasarathy Ramanujam: So uh I will be uh I mean as we discuss this particular change uh PR 948 today I will test it out uh to add that and then uh my focus over the weekend is to uh try to achieve P95 on uh existing setup to under 1 second. If we can do that I think uh we will have some stability. So uh after I have this uh image built, I will run with pure zon uh multi-ubnet nodes to see and uh uh continue to iterate over it over the weekend. Uh if uh I have achieved stability with that then I will uh include other clients and then start proceeding with what we uh can do on other fronts.
Gajinder Singh: Okay. So, I will continue looking at uh uh the devet runs, looking at the logs and seeing and see what I can infer from it. uh and I've continued reviewing the PR that you share with me and on DevNet 5 front basically continue tracking our progress uh on DevNet 5 shadow runs. So that's it. All right. Uh I think we are also over time or almost time because we started it as well. So any anyone else has anything they want to add? Okay. So all we all are are we all up to date on our plans for the next week? Sounds good. Cool. Thanks guys for being in today's call and we will see you on 5 June. And before that on the Wednesday call on 3rd of June.
Parthasarathy Ramanujam: Yep.
Gajinder Singh: All right guys,
Parthasarathy Ramanujam: Sounds good.
Gajinder Singh: take care.
Parthasarathy Ramanujam: All right.
Gajinder Singh: Bye.
 
 
Transcription ended after 00:50:52

This editable transcript was computer generated and might contain errors. People can also change the text after it was created.
