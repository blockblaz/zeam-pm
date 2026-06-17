Jun 5, 2026
Zeam call - Transcript
00:00:00

Gajinder Singh: Hello everyone, welcome to Zoom call. Today's June 5 and today we will look at the updates uh on the status of DevNet 4 and
Parthasarathy Ramanujam: Heat. Heat.
Gajinder Singh: our steps to move to DevNet 5. So before we move to DevNet 5 discussion, let's start with DevNet 4 discussion and try to wrap it up. So
Parthasarathy Ramanujam: So uh the same update that I had when I gave you on Wednesday because after that
Gajinder Singh: Pa
Parthasarathy Ramanujam: call uh clients have upgraded their uh dependency on lean VM and now everything is broken. Uh I can't uh achieve interop. So I tried uh just zmon only devnet and then ran into that uh error that occurred after our upgrade uh which I have resolved. Uh but I haven't run that yet because uh heeding to your uh uh advice or uh the other day I thought it's better we may probably start testing DevNet 5 itself rather than wasting more time on DevNet 4. Um so that's where we are. I didn't schedule another run. uh I can give a small local run uh to ensure the fix that was applied yesterday works.


00:01:23

Parthasarathy Ramanujam: Uh but yeah uh that's about it. But uh at the moment almost all DevNet 5 images seem to be unstable because each one is using their own commit hash of lean VM. So we need uh uh that to be sorted otherwise interop will start fading again.
Gajinder Singh: Uh so I think let's do the all ZME Devet 5 run not just the small one with all the nodes and see at least have good amount
Parthasarathy Ramanujam: Yeah,
Gajinder Singh: of finalization before we say that.
Parthasarathy Ramanujam: sure.
Gajinder Singh: Okay. So,
Parthasarathy Ramanujam: Uh,
Gajinder Singh: can you just remove
Parthasarathy Ramanujam: and yeah, I'll do that. And in parallel I was working on that zig lib P2P version in order to take advantage of that uh u I mean zk allocation thing that ML was talking about. It looks like even e lambda ran into some memory corruption issues when they added that. So if it's happening on a rust only client it might end up for us as well. Uh for now my zig lip p2p works with other I mean just zig implementation.


00:02:29

Parthasarathy Ramanujam: So I have run into some interrupt issues with rust. uh I will resolve that and then once it's stable uh I'll uh include that in the ze code base and then we can change yeah for
Gajinder Singh: So maybe we targeted net 5 uh right now just net 4 runs and
Parthasarathy Ramanujam: dev
Gajinder Singh: uh yes no need to move to uh the optimized uh zcalo
Parthasarathy Ramanujam: network.
Gajinder Singh: version if there is some other client that is trustbased client that is facing corruption issues. So can we just make sure that our current uh code is stable and there are no
Parthasarathy Ramanujam: Yeah,
Gajinder Singh: other issues. I mean by tomorrow morning we have I want to merge Devet 5
Parthasarathy Ramanujam: sure.
Gajinder Singh: PR and O wanted to publish some stats about
Parthasarathy Ramanujam: Okay, I'll do that.
Gajinder Singh: DevNet 4. So we do have some stats about DevNet 4. Basically all the stats that you were collected collecting regarding the aggregator performance. So we do have them right.
Parthasarathy Ramanujam: Yeah,
Gajinder Singh: So, can
Parthasarathy Ramanujam: we do have uh the latest.


00:03:30

Parthasarathy Ramanujam: Yeah, I'll share that with you. But the the only difference is the metrics that I had requested on missed uh aggregation slots
Gajinder Singh: we
Parthasarathy Ramanujam: that's not yet available on all clients. So, we haven't actually tested one. We can do that with uh DevNet 5 probably.
Gajinder Singh: Yeah, we can do that with DevNet 5. that uh we basically do we should publish numbers on devet 4 as requested
Parthasarathy Ramanujam: Sure.
Gajinder Singh: by and u so just do first of all all zim run and confirm that we are in a stable position all right
Parthasarathy Ramanujam: Okay, I'll do that.
Gajinder Singh: uh let's move to five discussion yes sky you wanted to sayKai Chen: Uh yeah. Uh I uh I open uh PR 9 76. Uh yeah. So I think it it can uh it can benefit for the docker environment because there is there is some problem with the z get CPU count. So
Gajinder Singh: Okay.
Kai Chen: maybe
Gajinder Singh: So, this seems like uh Ziggly P2B, right? So,


00:05:01

Kai Chen: no.
Gajinder Singh: your PR I'm just checking it bumps the version on Ziggly P2B.
Kai Chen: Uh this this is not too
Gajinder Singh: No, this is not your PR.
Kai Chen: late.
Gajinder Singh: Which which 979?
Kai Chen: 9 uh 76.
Gajinder Singh: Okay.
Kai Chen: Yes. Uh
Gajinder Singh: Yes. Yes. I got it.
Kai Chen: yeah. So maybe in the Docker and the Kubernetes environment there is a wrong CPU count we get. So maybe it
Gajinder Singh: Okay. Right. Right.
Kai Chen: does.
Gajinder Singh: Because you want what has been allocated to the container, right?
Kai Chen: Yes.
Gajinder Singh: So this probably makes sense. Uh let I think it's a small PR that we can merge in PA. Just have a look and merge it in as well before you run your uh odd sim 5. All sims deet 4.
Kai Chen: Yeah. And uh and I found another problem. So you can't see the line 77. So so currently we most use the the global single thread and so from the from the comments for this uh


00:06:45

Gajinder Singh: All
Kai Chen: function I think it's it's not uh it should not be used in production code is that the command says yeah it it need only use for
Gajinder Singh: right.
Kai Chen: debug purpose. So, so I think we we need change to um most uh IO from the May. There is uh there uh there is uh init IO inject from the May function that is uh so that is default IO for the application to use but but this this has some uh comp comp uh compile issues. So maybe I need to take a look at
Gajinder Singh: So the fallback is one is the debug one I no. So, so if you want to test on the if you want to debug
Kai Chen: So
Gajinder Singh: with threaded the global single threaded if you want to test on that
Kai Chen: yes,
Gajinder Singh: so how so you need to inject it where you need to inject it in build
Kai Chen: no, there is already an inject from The main functions we can get it from the init IO that's the standard usage for the application.


00:08:48

Gajinder Singh: Okay. So you have installed over there and uh so any io will will it default to debug in debug single threaded instance in debug or what let's say you want to use so what is the behavior over here so when you do process IO install init.io So how how does it default to global single threaded
Kai Chen: I'm sorry.
Gajinder Singh: so let's I mean this is for for the logs
Kai Chen: No, I think we we uh a lot of code use that use the global thread IO right now. So, so I think most most should be not used state.
Gajinder Singh: So do you think that there are any difficulties that might come up for debugging or this is just fine?
Kai Chen: Um, so I just need to make uh make the uh I just need to check why the the unit test failed right now.
Gajinder Singh: Okay. Yep. Just get the unit test working and let's Okay. I mean pa don't wait for this particular PR to test it out because but yes on this PR is merged then


00:10:40

Parthasarathy Ramanujam: No, no. Yeah. I'll I'll merge the CPU one and
Gajinder Singh: let's yeah when this PR is merged then also list it let's test it out
Parthasarathy Ramanujam: Yeah.
Gajinder Singh: on local as well and uh but yes get
Parthasarathy Ramanujam: Yeah.
Gajinder Singh: your multi-node multi- machine network going on even before All right, cool. Uh, do we have anything on DevNet? Anything else on Devet 4? Okay, so let's go to DevNet 5. So on that Kai I wanted to ask the shadow branch that you have uh main PR or main PR. So we have basically introduced a new CI uh that rebases that branch on the main branch but right now it's failing because I guess because the dev shadow branch is on devet 5, right?Kai Chen: uh no actually I I create another right now I create another branch net 5 shadow because because the Uh I think a lot of conflict I made. So I uh before this PR create I already create another branch. So uh I think in future we uh we can we can uh we should we should avoid this.


00:12:33

Kai Chen: So I I I can uh I can take a look at how to how to make it um smooth.
Gajinder Singh: Yes. So I I think once we have DevNet 5 in and you change the shadow branch to your shadow devet 5 branch, right? So each time we should be able to rebase the shadow branch. So so the shadow branch already should be based should be based on the main
Kai Chen: Yeah.
Gajinder Singh: branch and commits on top of it. Right? So there there is rebasing is a strategy that you are applying nu
Kai Chen: Yeah.
Gajinder Singh: right.
Kai Chen: Yes.
Noopur Singh: Yes. Yes.
Gajinder Singh: So rebasing strategy will basically make sure that that diff whatever is between uh the main and the shadow branch that will continually basically keep on getting applied to the head of the main. So if generally we are not touching on any of that code then there shouldn't be any conflict but if there are conflicts basically we have to resolve it and that's why we have this new notification that tells us that there is a conflict while replacing the shadow branch.


00:13:54

Gajinder Singh: Regarding shadow, uh the server is up. It is I basically upd how many cores? It is basically Intel 14th generation i7. So it should have it has 25 cores. No, 27 28. So, it has 28 cores and it should be sufficient. It has 96 GB RAM. So, uh what I'll do is I'll just clean it up a bit and ask NU to provide you access for it. So, you should be able to SSH into it and then be able to run whatever you want to run.
Parthasarathy Ramanujam: uh how many uh nodes are we targeting the
Kai Chen: Okay.
Parthasarathy Ramanujam: generation
Gajinder Singh: So that depends upon the memory right. So whatever we can fit in 96
Parthasarathy Ramanujam: 96. Okay.
Gajinder Singh: GB.
Parthasarathy Ramanujam: So but shadow only it's
Gajinder Singh: So it says 96 but 90 90 GBs will be available because it currently
Parthasarathy Ramanujam: short.
Gajinder Singh: is saying 94 GB and then 1 GB or whatever it takes to for the processes.
Parthasarathy Ramanujam: Okay.
Gajinder Singh: So assume that whatever you can fit in 90


00:15:24

Parthasarathy Ramanujam: Right.
Gajinder Singh: GB.
Parthasarathy Ramanujam: But this will only simulate or provide us an idea of how it would behave in a network. Right. not necessarily uh uh the CPU
Gajinder Singh: Yes, it is it is it is a it is a simulation and the simulated time is not equal to the real time.
Parthasarathy Ramanujam: load.
Gajinder Singh: That means that your real time or doing all these things could be much much higher. For example, you can say that you took 3 seconds for aggregation but you can move the shadow clock to only by only 4 second. So you understand that.
Parthasarathy Ramanujam: Yeah. No, but in this case there should we should add uh some sleep time even for normal validators as well, right? Because at the moment I believe Kai's uh thing had sleep only for aggregators if I'm not wrong.
Gajinder Singh: Yes.
Parthasarathy Ramanujam: Um
Gajinder Singh: So we will need to add sleep for proposal or wherever we think uh in duties we take quite a long amount of time. So you can look at the metrics that we have and try to introduce sleep of that that amount on those particular things.


00:16:36

Parthasarathy Ramanujam: Okay. Okay.Gajinder Singh: So each time you are for example processing a destation you can you can say I want I want to sleep this much time. Each time you are processing an aggregate you can say I want to sleep this much time. Each time you are processing processing uh proposal you say I want to sleep this much time. Each time you are doing uh accepting new votes basically each time you're running interval you can say I want to sleep this much time in this particular interval. So we we basically need to have all these plugs that we can configure and then we can run the network on top of that. But as such yes uh try running it and uh once you have the access if everything works basically then we introduce all these sleep metrics so that we can simulate it properly.
Kai Chen: Oops.
Gajinder Singh: also the RAM is clocked at 7,000 MHz. I don't know if that will help or not but it is yes one of the top 10. All right.


00:18:32

Gajinder Singh: Uh Angel, what do you have for
Anshal Shukla: Uh so like my updates are like mostly around DevNet 4 aggregation timings and stuff. Uh apart from that the clone PR got merged both on F3 side and on Z side. And apart from that like the tree root hashing also got merged. Uh I'll update that branch uh uh the commit on Zoom side as well. That is yet to be done. uh I'm already working on like the flag so that we can uh manipulate like the number of aggregates that we want to perform as an aggregator. Uh right now how I'm doing is uh a bit greedily. So it starts from my localized finalizer state and it tries to build upon that and select the restation data uh as per like my localized uh finalized block and uh it builds basically on top of that. So if I select a attestation data based on source and target uh the next attestation data will basically be selected based on the based on this selection. So initially I'll select like the best I can find u as to my localized final and I'll just build upon that.


00:19:52

Anshal Shukla: So I'll raise that today. Apart from that like I'll also review guys devet 5. Uh yeah apart from that like I was involved in running some of the test cases alongside like the mix and match of different nodes different client so that we can keep that Just wake up.
Gajinder Singh: Got it. Do you have anything for us?
Noopur Singh: Um no I was out for most of the week and uh then I finished the CI shadow CI work and uh currently I am trying to like um uh sort the issues on Zen. So like there are multiple issues that need closing or uh or are not applicable anymore. So I am going through all those. So that's it from me.
Gajinder Singh: in this guy.
Kai Chen: uh yes for for DENTE 5 I I think so in the in the DENT 5 channel in PQ top of TG group. There is there is a new spike chike change which changed the side block proof from from a bite list to uh container uh SSD type. So it uh it will impact the hash tree root.


00:21:38

Kai Chen: So, uh I
Gajinder Singh: Why are we doing that? I mean, did we not decide not to basically not to do that password? I remember we decided against it right in the calls.
Parthasarathy Ramanujam: uh hashtree root was changed uh one uh we merged that PR right uh Vijender you said that for now we can do it uh just for optimization even though this will change in DevNet 5 uh you remember Anel had commented one uh and you said we can merge and test this for now but it'll change in DevNet 5 so we might have to uh undo that particular commit alone
Gajinder Singh: No, I think Kai is talking about basically representing signature in a SSD container, right?
Parthasarathy Ramanujam: Oh, was that uh I don't remember if I uh Yeah, Kai, if you can just tag me on that comment, I will see and I can uh we can uh remove that.
Kai Chen: No, no.
Parthasarathy Ramanujam: Oh, this is on the spec.
Kai Chen: I mean there Yeah. I mean there is a spark change.
Parthasarathy Ramanujam: Okay.
Kai Chen: So I'm not so it it's already merged.


00:22:48

Parthasarathy Ramanujam: Okay.
Kai Chen: So
Parthasarathy Ramanujam: This is not zoom. Uh this is spec uh changed byKai Chen: So yeah. So, I'm not sure if I need to uh make this change
Parthasarathy Ramanujam: Toma.
Gajinder Singh: So this this is only changing the names,
Kai Chen: here.
Gajinder Singh: Right? This is not changing the type. This is not putting the container type in there. Right? This is just you can just see the renaming of
Kai Chen: H I think it change not only name change
Gajinder Singh: it.
Kai Chen: um proof proof type.
Gajinder Singh: basically can and until can you look into it so my understanding is they should not
Parthasarathy Ramanujam: confidence. Thank you.
Gajinder Singh: be changing the signature container type it should just change the name and this is what I'm mostly seeing or they they might have changed the name of the types right so but that does not change the SSD
Kai Chen: Okay.
Gajinder Singh: structure as such SSD structure will only change if we have a signature container that has then properties.


00:24:08

Gajinder Singh: Right now signature is uh opaque bytes for us. So unless they are changing from opaque bytes to something that is
Kai Chen: Okay. Okay.
Gajinder Singh: uh that has properties then it's a problem.
Kai Chen: Okay.
Gajinder Singh: Otherwise,
Kai Chen: Okay.
Gajinder Singh: it's just a name change and we can also do a name change because yes, it's good to be on the same name parity as the
Kai Chen: I will I will update my PR for the name
Gajinder Singh: spec.
Kai Chen: change.
Gajinder Singh: Yeah. Yeah. And just confirm that it just the name change.
Kai Chen: Okay.
Gajinder Singh: It looks to me name change, but yes, I'm looking at a very high level.
Kai Chen: Okay.
Gajinder Singh: So my update is that uh yes I have been looking at PRs looking at DevNet for debugging and uh now hoping that we move to DevNet 5 and start debugging that. So that is my update.
Parthasarathy Ramanujam: Okay. Uh I I just have a naive question just trying to understand DevNet 5 uh I mean how it works operationally.


00:25:20

Parthasarathy Ramanujam: So uh proposal is chosen uh in round robin and uh during the proposal slot it aggregates all of its uh the block data. Uh can an aggregator also be a proposer? Uh so would then there be two uh instances within a given slot uh where the aggregator would have to aggregate both block as well as attestation signatures.
Gajinder Singh: Yes, short answer is yes, it can be.
Parthasarathy Ramanujam: Okay. So in
Gajinder Singh: So if aggregator can have or cannot have validated duties,
Parthasarathy Ramanujam: theory
Gajinder Singh: right? So we we created aggregator in a way that it can run independently and it can have
Parthasarathy Ramanujam: Yeah.
Gajinder Singh: or not have validated duties.
Parthasarathy Ramanujam: Okay. Um I I'm just thinking if we should introduce a metric just to understand if because of an aggregator role an aggregator is losing its validator uh slot by not completing on time given the hardware requirements.
Gajinder Singh: Yes. So,
Parthasarathy Ramanujam: We
Gajinder Singh: so basically aggregator should stop aggregation by the time I mean by the time new terms,
Parthasarathy Ramanujam: yeah it it should but


00:26:35

Gajinder Singh: right? So, it should already so we we can have a metric that says that was aggregator able to complete its duty on time or not the aggregation on time or not.
Parthasarathy Ramanujam: Okay.
Gajinder Singh: If we don't have that particular
Parthasarathy Ramanujam: So,
Gajinder Singh: metric
Parthasarathy Ramanujam: so in which slot interval do uh does a validator start to uh aggregate the blocks? Uh is it slot one or zero and
Gajinder Singh: zero zero only slot zero is for proposal.
Parthasarathy Ramanujam: Okay.
Gajinder Singh: right now. So what I was discussing in the call was that uh proposal
Parthasarathy Ramanujam: So,
Gajinder Singh: aggregation can happen like in the pre the last slot of the previous in the last interval of the previous slot as well.
Parthasarathy Ramanujam: right. I mean,
Gajinder Singh: SoParthasarathy Ramanujam: what happens now is aggregator is slot uh two, right? Uh so that spans up to uh slot f uh I mean sorry interval two, right? uh it spans up to interval four uh on few cases and if proposer duty can start aggregating from sl interval flow


00:27:35

Gajinder Singh: Yes.
Parthasarathy Ramanujam: four um then uh immediately after an aggregation of attestation signature finishes the aggregator would start to aggregate the block uh data as well.
Gajinder Singh: So you need so you need to run all these duties with a timeout.
Parthasarathy Ramanujam: So there
Gajinder Singh: Right? Obviously, you can't go into the timeout of the other slot.
Parthasarathy Ramanujam: yeah
Gajinder Singh: You have to basically throw away whatever you are doing and be ready for the next
Parthasarathy Ramanujam: be ready for the next one.
Gajinder Singh: slot because that means you were not able to aggregate
Parthasarathy Ramanujam: Yeah.
Gajinder Singh: simply means that and
Parthasarathy Ramanujam: Okay. Okay. Let me add that metric just to measure. Uh but uh yeah I mean for
Gajinder Singh: right so right.
Parthasarathy Ramanujam: the
Gajinder Singh: So in any case, aggregator should stop its work basically when the third slot is over,
Parthasarathy Ramanujam: third slot is over.
Gajinder Singh: right?
Parthasarathy Ramanujam: Yeah but it at the moment it's not uh at least that's not what we observe in the uh implementation across clients. Uh but yeah uh I mean in general


00:28:49

Gajinder Singh: So we so for other clients, we don't care, but for Zen, let's put this time out, right? So when you're on aggregating
Parthasarathy Ramanujam: yeah on on Zen if if we don't do it we stop and that's why we we miss uh aggregation on certain slot uh which is fine.
Gajinder Singh: Yeah,
Parthasarathy Ramanujam: Uh
Gajinder Singh: that is fine. That's fine.
Parthasarathy Ramanujam: yeah
Gajinder Singh: That is how it is supposed to work here.
Parthasarathy Ramanujam: in for redundancy maybe we should introduce multiple aggregators per uh subnet. Uh but this is something we can do during DevNet 5
Gajinder Singh: Yeah. So, ideally in the real network you will have multiple aggregators per subnet.
Parthasarathy Ramanujam: testing.
Gajinder Singh: uh but right now we don't because we want to test the aggregator performance and this has zoomed down into what the issues are.
Parthasarathy Ramanujam: Yeah.
Gajinder Singh: Ideally yes we will have multiple aggregators a subnet and there could be actually someone who is aggregating everything right so it is like again a pear kind of thing there could be a super node that is aggregating everything which is basically some mostly run by ethereum foundation so ethereum foundation has super nodes that are there to smoothen over pas right and And there again the big entities like Ethereum Foundation,


00:30:09

Parthasarathy Ramanujam: right?
Gajinder Singh: Bitine, Lido or all these people right who wants to have a good backbone of the network they can basically put in these uh hardware that ensure that everything is good.
Parthasarathy Ramanujam: I know.
Gajinder Singh: Even the builders can do that right. So builders would not want their blocks to fail and to not be part
Parthasarathy Ramanujam: Yeah.
Gajinder Singh: of uh the chain right of the canonical chain. So it is very likely that builders will um these aggregators so that they can make sure that stationations for the blocks can be backed can be in the
Parthasarathy Ramanujam: Yeah.
Gajinder Singh: gen.
Parthasarathy Ramanujam: Um and the other question is more on the pro proposed EIP you wanted uh Unell to work on
Gajinder Singh: Yes. So yes. So the last item that we want to discuss is the EIPs that we should start
Parthasarathy Ramanujam: and
Gajinder Singh: writing and one of the EIPs is about the key format key store which path is taking lead on right and the other PR is about
Parthasarathy Ramanujam: Yeah.
Gajinder Singh: uh other EIP about the aggregator role that we want to add.
Parthasarathy Ramanujam: Yeah.Gajinder Singh: So Anchel, do you have any doubts or questions about this? Because this is where I want you to take the lead on and write up the
Anshal Shukla: Yeah,
Gajinder Singh: CIP,
Anshal Shukla: I I look into it.
Gajinder Singh: right? And basically we will do this in coordination with the team so that we have an EIP with a broader support. All right. Anything else do we have to discuss? 3 2 1. All right.
Parthasarathy Ramanujam: No.
Gajinder Singh: Time up.
Parthasarathy Ramanujam: Cool.
Gajinder Singh: All right. See you guys next week on 12th June. Unless you guys are going on holidays. I'm not. So you you don't have holidays over
Parthasarathy Ramanujam: is holidays.
Gajinder Singh: there.
Parthasarathy Ramanujam: Uh, no.


Transcription ended after 00:32:49

This editable transcript was computer generated and might contain errors. People can also change the text after it was created.
