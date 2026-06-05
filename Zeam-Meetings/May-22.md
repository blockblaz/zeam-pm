May 22, 2026
Zeam call  - Transcript
00:00:00
 
Gajinder Singh: Hello everyone, welcome to May 22 zinc call. Today we will discuss DevNet 4 uh uh scaling efforts, optimization efforts as well as shadow runs and our plan for shadow runs and a little bit about DevNet 5 readiness. So let's start with quick updates uh on DevNet 5 readiness from Kai because then we'll focus rest of the call on DevNet 4 runs and shadow runs shadow sims.
Kai Chen: Uh yes. Uh this week I was working on the uh implementation of uh de 5 um almost almost finished but uh um but I found uh theme test failed. Oh, so check so checked with May branch. Uh it seems um the sim test right now has some flaky uh fa failing. So uh passa pa submit uh new PR. So maybe I I will look at it uh later.
Gajinder Singh: So when do you think uh we'll be able to have the implementation for the net
Kai Chen: Um, I can uh I need maybe I can uh submit it today or tomorrow. I need to do some review.
 
 
00:02:03
 
Gajinder Singh: All right, Angel, can you coordinate with Kai on whatever hurdles he's facing so that we have a Quick definite f implementation. All
Anshal Shukla: Yeah, sure.
Gajinder Singh: right,
Anshal Shukla: I'll do that and as soon as like the PR is also ready, I'll look into it.
Gajinder Singh: sounds good. All right, since we have camel here, so let's move to uh devet for sims with shadow. So with regard with regards to that, I'll just present it to Camille to sort of take the lead on this. So Kamill where do you think we are currently?
Kamil Salakhiev: Uh yeah, I was playing with uh basically shadow and link quick start since
Kai Chen: All
Gajinder Singh: Hey,
Kamil Salakhiev: yesterday. Um yeah, as I wrote in the chat,
Kai Chen: right.
Kamil Salakhiev: I think um yeah, basically the main question was uh okay, I say we want to run large scale simulation starting from 64 nodes. Uh how large we can scale? And the question was like how much RAM um Zim and other clients would consume so that yeah we can estimate how large server we need for the simulations.
 
 
00:03:22
 
Kamil Salakhiev: Um yeah so from what I see I think I wrote in the chat like Zim consumes around 400 megabytes per node if it's not an aggregator. If it's an aggregator then around 600 megabytes. Um yeah, I managed to do simulations uh using link quick start. I think simulation lasted like um say six minute of simulated time. Um so yeah I think it's based on that you can estimate how much we can u yeah how large we need to have a server. uh what I observed well yeah today I also managed to run link start so that we have like zim working together with clean under shadow um so basic yeah so everything worked progressed um that's good um so yeah based on that yeah depends on like if we want to say have like 100 nodes so we can estimate um so like we need maybe uh 60 gigabytes. Yeah, if we have like 64 GB server, that would be good enough, I believe, like with some margin uh to run maybe 100 nodes simulation. Um yeah, that's that's already good.
 
 
00:04:44
 
Kamil Salakhiev: I mean, there are a few things that we need to I mean, the thing that Zim and clean u can run under shadow, that's great. Uh that that's a good start. What we need to do next is well as I can see it is um yeah for now we're just testing uh I I don't know like I I didn't check carefully uh what exactly uh the underlay network we use when we simulate a shadow most likely it's just simple maybe one Gbit network uh this is not realistic so we can with shadow uh we can do stuff like uh to have there like EIP787 or requirements or if we want to bump them. Yeah, we can do this easily as well. Uh that's regarding bandwidth requirements per node. Another question is uh latencies between nodes. Uh there are a few options what we can do there. U yeah I would start maybe the simplest one is just geographical latencies uh uh between nodes. Basically what is that is yeah it's a we have a data which is like simple JSON file which we use in P2P team basically the latency between countries so that inside countries the latency is very small uh it could be just be even one millisecond um up to maybe 30 millisecond then between countries yeah it depends on like real distance and then we have like the uh distribution uh we know the distribution of Ethereum nodes like in which countries we have how many nodes and that then we use the distribution to kind of like yeah with
 
 
00:06:29
 
Kamil Salakhiev: certain probability assign nodes to certain countries and then yeah that uh strategy could also work to define latencies another one yeah we have also right atlas database it's a bit outdated because I think it's from 2018 but it also had some data for latency between nodes so we can s sample from there as well um so yeah anyway that's one thing another thing is um uh what we need to introduce for proper simulations is uh so that just just for everyone to understand who didn't work with shadow before the shadow is a uh discrete event simulator and it's focused on networking when we do u everything besides so it's basically uh it simulates system sys calls and uh the times moves forward uh well for Example uh if we do say aggregation like for example signatures aggregation from shadow perspective uh it will look like instantaneously I mean like it's just less than one millisecond it's just 0 millcond for for the aggregation because um yeah it doesn't it it doesn't simulate compute time that was that would be taken in reality. What we should do instead uh instead of we should add some artificial sleeps uh say before um well some heavy operations like uh signatures aggregation yes aggregation verification of signatures it's usually pretty cheap less than one one millcond I believe but yeah anyway that's also uh would be great to have um ideally if we could have this um I think in clean we did this so that it's more like uh environment variable and then when we build clean with the
 
 
00:08:24
 
Kamil Salakhiev: shadow config enabled yeah it's basically just sets these uh environment v it's basically puts them into binary this is not super convenient ideally if we could have like say either environment variable or cli flag so that we can dynamically change uh like what is our signature aggregation rate or what is our verification rate segregation rate whatever um and then Yes, that would be very convenient. So that then we can simulate really real network. So that we can say like for example we want to run an experiment like say we have 64 nodes network or even 100 nodes network. Uh we say we have like that many aggregators we have that rules for latencies that rules for bandwidth. Uh this is our signatures aggregation rate and so on so on. And then yeah we can play with these parameters and see how um and see how yeah this basically progresses well howization is affected and how attestations are propagated. Um yes to check how the stations and blocks for example propagate. Yeah, we need just to ensure that we have proper logs and then we can collect these logs.
 
 
00:09:31
 
Kamil Salakhiev: Uh um cloud is very good with this. I mean we can just uh then once we have the output data we just can ask like hey uh what was that stationation propagation and so on. Um so yeah that's that's what we can do for now. Uh what would be great if for example yeah since this is Zim call but it's basically for any clients that want to uh run uh simulations under shadow would be great to have this some flag either CLI flag or envirs to kind of yeah set some artificial sleeps before aggregations and other operations which I described. Um other than that I think we're good. Uh yeah. So yeah, we can run simulations both under uh ARM because I patched shadow so that it runs in ARM and also in AMD. I have 64 gigs AMD server. By the way, simulations run much slower there in comparison to uh Mac I mean in ARM. Uh so that's just also something to keep in mind. Uh but simulations work in general.
 
 
00:10:42
 
Kamil Salakhiev: Um but if we have uh yeah sorry I will stop here because yeah maybe speak too much if you have any questions
Gajinder Singh: So if if I understand how so so there is is there no actual computation
Kamil Salakhiev: or
Gajinder Singh: that is happening what do you mean when you say sleep maybe we'll just go and dig into
Kamil Salakhiev: uh
Gajinder Singh: details of shadow I haven't done it so far uh so so are there actual computations happening right like aggregation Right.
Kamil Salakhiev: what kind of competitions between
Gajinder Singh: Aggregation and
Kamil Salakhiev: Uh no yeah yeah there is no uh real well it's kind
Gajinder Singh: then
Kamil Salakhiev: of real aggregation is happening but from shadow perspective from simulated time perspective it takes less it just takes zero milliseconds u yes and uh that's not realistic of course um but that's actually that's good from shadow I mean say we have like 100 nodes and if yeah that oh yeah now I get what you mean by competition. So yeah, if we like have 100 nodes, they all need CPU resource. Uh then if it was like in real time, yeah, then it would not be super realistic.
 
 
00:11:55
 
Kamil Salakhiev: Here they intercept system calls. So we um yeah, it's basically can happen some it's in fact happen sequentially. So then yeah we have some aggregations happen aggregation happen on by some node but in reality from simulated time perspective it just takes 0 millconds so it's um it's fine yeah
Gajinder Singh: Got it. Got it. So basically real computations are happening but shadow is not taking in that time because that simulated time is zero for it.
Kamil Salakhiev: Yeah. Yeah.
Gajinder Singh: Got it? So I mean this is just about the nodes or if we put more validators on each of these nodes then the computation time will obviously scale because aggregators will need to be aggregators will need to work a lot. So my question is what what is the kind of network that we should target to simulate
Kamil Salakhiev: uh by yeah we yeah it's it's
Gajinder Singh: and
Kamil Salakhiev: easy to get confused here. What do you mean by nodes? Do you mean like each validator in fact can run uh well multiple validators?
 
 
00:13:03
 
Kamil Salakhiev: They might be kind of hosted on the same node. Is that the behavior that you interested in simulating or
Gajinder Singh: Yes.
Kamil Salakhiev: what?
Gajinder Singh: So like for example in when we run lean quick start these devets one node can have four validators or 100 or 200 right. So,
Kamil Salakhiev: Yeah.
Gajinder Singh: so let's say if we want to simulate maximum number of validators load as well even though we might not explore the number of nodes we can say that okay there are
Kamil Salakhiev: Yeah.
Gajinder Singh: 600 200 I don't know whatever nodes around the world simulated around the world and they have let's say thousand validators each and that means that we can simulate like 60,000 validator network which is how these things are which is how actually Ethereum network is Average number of validators per node could be really
Kamil Salakhiev: It is. It could be. I'm just Okay.
Gajinder Singh: high.
Kamil Salakhiev: It's a debatable like how much we need to account for this because there are ongoing efforts in kind of consolidating validators uh right now uh so that yeah we have like maxv and so on so that yeah uh large node operators they did not in fact run multiple validators but they just merge them into a single uh logical validator um so both logical and physical so yeah that's um we hope and there are in future Sure.
 
 
00:14:28
 
Kamil Salakhiev: Yeah. The well I I think in the road map we have goal to have like 128k validators. Um so that yeah we kind of want uh large operators to consolidate and also their ongoing efforts like in batching at stations but this is
Gajinder Singh: So bas basically have having these many validators will also sort of increase the network load in the subnets right as well as because that that much
Kamil Salakhiev: Yeah.
Gajinder Singh: attestation data is going in. So I I think it is a bit relevant and can give us in important insights with that regard. Okay, what is the subnet sizing that we want? uh so it could be worthwhile to also pump up uh the
Kamil Salakhiev: He's so
Gajinder Singh: per node validated count in the simulations. My question is basically you know what what is the kind of architecture or the network that uh we we try to simulate and what is the target of the end results that we want from it. Right?
Kamil Salakhiev: All
Gajinder Singh: So if we basically define this before we start these experiments now that we
 
 
00:15:34
 
Kamil Salakhiev: right,
Gajinder Singh: know that Zim and Keen are capable of running under shadow. So if we define these targets so let's then we can basically say that okay this is the kind of network we are trying to uh simulate and these are the results for it.
Kamil Salakhiev: Um yeah, if you asking like what kind of network if you are you asking about uh like um yeah uh topologies or uh or what like or like how many uh validators per node we should have we should target. Uh yeah to be honest there are a lot
Gajinder Singh: It is sort of a mixed version, right? So I mean it is like uh yes, we need to break it down and figure out how to go about all of these particular configurations. In terms of topology, I think the current topology that we are following is the subnet based topology.
Kamil Salakhiev: Yeah.
Gajinder Singh: So then how many subnets or what is the subnet size that we should be having uh so that basically an aggregator can handle it or what kind of aggregators do we need uh do do they need to uh aggregate across different subnets?
 
 
00:16:48
 
Gajinder Singh: So or basically another variant of the same question is how many subnets do you think a normal typical aggregator should aggregate and what is the redundancy of the aggregators that we want in a particular subnet right so these are all the questions that come up to have a production kind of network running and that is the main aim to collect all this data with this particular sim so that we can say that okay this is
Kamil Salakhiev: Yeah.
Gajinder Singh: the configuration that we should target for long running devets or a public test
Kamil Salakhiev: Yeah. I mean we can well we can test this. We can simulate.
Gajinder Singh: net.
Kamil Salakhiev: I mean uh if we have any client Yeah. Maybe I I I missed that. But yeah, do we have like this functionality in the net 4 so that an aggregator could be assigned to multiple subnets? Uh okay.
Gajinder Singh: Yeah.
Kamil Salakhiev: Yeah.
Gajinder Singh: So we we have
Kamil Salakhiev: Then as long as we can configure that, yeah, it's not a problem. Yeah, we can test
 
 
00:17:47
 
Parthasarathy Ramanujam: No, but the problem is even with a single subnet we are not able to achieve finality.
Kamil Salakhiev: this.
Parthasarathy Ramanujam: That is when one aggregator per subnet uh listening to attestations on the same subnet it is part of uh we still facing issues. So it's too early to probably make an aggregator listen to multiple subnets. But I believe Gajendra's question is uh I mean we ought to provide a uh a requirement to uh EF in order to specify uh for a longunning DevNet. How many machines do we need or how many servers we can test this on? For that we would need to use uh the shadow simulations to arrive at some kind of uh uh estimate of how many machines we would need what would be the monthly budget or something of that
Kamil Salakhiev: Mhm.
Parthasarathy Ramanujam: sort.
Kamil Salakhiev: Right. Uh
Gajinder Singh: So yeah, that that
Parthasarathy Ramanujam: I mean even before we go that the first sorry uh even before we go there the first thing is in order
Kamil Salakhiev: yes.
Gajinder Singh: is
Parthasarathy Ramanujam: to run shadow simulations on a server how big the server should be.
 
 
00:18:49
 
Parthasarathy Ramanujam: So that's the first estimate we have to
Kamil Salakhiev: which if I if that was a question for me,
Parthasarathy Ramanujam: provide.
Kamil Salakhiev: I would say that 64 gigs should be good enough for now. Uh or the obviously the more cores we uh how the RAM
Parthasarathy Ramanujam: Okay.
Kamil Salakhiev: the the larger the RAM, the larger the simulation, the more nodes we can run and then the more CPU we have on this server, the faster each simulation or the less time it will take. Uh so uh
Gajinder Singh: Okay.
Parthasarathy Ramanujam: Okay.
Gajinder Singh: So, so what what I understand Camil is that CPU is not that much of of a concern because whatever is the compute time it does not matter with regard to shadow right and uh you can take 2 seconds to do an aggregation and on shadow we can say that okay it was well under8 second right because they are all system calls based where you intercept it and uh artificially move the time
Kamil Salakhiev: Yes. I mean each simulation each simulation could take uh some long time.
 
 
00:19:52
 
Kamil Salakhiev: I mean say we have we want to simulate five minutes of uh simulated time which is like yeah like say five minutes of the network time. Uh if want to simulate that the in real world clock time it could take like it depends. Yeah, to be honest, I didn't test that on AMD server yet. Like this large simulation on my Mac, it took like maybe 15 to 20 minutes uh for 64 nodes I believe. U but on IMD it was much slower. So it might take an hour for a single simulation. But still I mean we can do some kind of I don't know launch auto research for example like just play around with parameters and rerun simulations constantly. Uh ideally uh uh shadow is discrete event simulator so we can just it has seed parameters so that we can change seed um and then uh it's not works perfectly every time but yeah if our clients they do not have any um well if um if they're truly deterministic if there is no like some sort some unclear source of randomness that shadow cannot intercept cept then uh yeah all simulations they behave exactly the same between all runs.
 
 
00:21:16
 
Kamil Salakhiev: I mean given the seed given all the parameters uh and we if we want to reproduce for example yeah we face some issue okay with this seed with this parameter we notice that finalization stalled and then yeah we can reproduce that debug add more logs and the behavior should be the same um that so yeah what I'm saying is yeah even if simulation is taking long time yeah we can um I mean we have a server we just left leave it running uh maybe then create some kind of observatory so that yeah we can see like then we just wake up in the morning and check uh what happened during the night how finalization was progressing and so on.
Parthasarathy Ramanujam: So I I just shared a link for you and Gajender from Hedgener.
Kamil Salakhiev: Yeah.
Parthasarathy Ramanujam: They offer uh server auctions which are uh older configuration servers with higher RAM but they don't charge a one-off set fee in this. Uh so if there is a configuration here that you like we could probably place an order for it and we use it only as long as we need it.
 
 
00:22:24
 
Parthasarathy Ramanujam: Um maybe so you have uh in uh the
Kamil Salakhiev: Uh okay.
Parthasarathy Ramanujam: chat uh go in this chat I shared
Kamil Salakhiev: Yeah. If this is really a for limited time,
Parthasarathy Ramanujam: it.
Kamil Salakhiev: we should kind of allocate this time wisely. So maybe first create um yeah luckily I can I yeah I I have my own server so yeah I just create some scripts there.
Parthasarathy Ramanujam: Okay.
Kamil Salakhiev: what I would really love to do uh yeah uh what would also give us a really nice picture is um yeah it's 2026 again with the help of AI we can visualize really nicely what actually going on in the network. Um if you saw yeah you've been gender you've been in the can Raul showed this demonstrations for X-ray where it's basically we fetch data from all gossip subtopics within the slot. So we see how each slot uh during each slot like at at which uh like uh at what period of time we had like the spikes in bandwidth. Uh so this could be visual visualized really nicely.
 
 
00:23:34
 
Kamil Salakhiev: Uh and also we have thing like net v which gives us really nice picture of how for example attestations propagate. Um yeah what I really want is uh yeah take some time and uh work yeah uh work on kind of visualizing this and it should not be too complicated. Yeah because we have already net vis we have x-ray. Yeah, just uh um yeah, patch some client. It could be clean, could be Z, whatever. Uh and then uh yeah, and then yeah, once we have a server, yeah, we run the simulations. Yeah, like with different amount of nodes. Uh we collect all the data and then we can put them on some website so that yeah, we can see actually this everyone understand like where we actually spend the bandwidth. Uh and that also kind of gives us the picture of like yeah where we should improve next what are the bottlenecks. Uh net viz would give us great picture of like for example we have gossip sub gossip sub has problem with uh duplicates so that yeah the bandwidth is not consumed twisely so that we can receive same message multiple times u all of that stats.
 
 
00:24:48
 
Kamil Salakhiev: Um I would honestly I would first like I have 48 gigs uh on my MacBook so I can already like play around with uh uh 64 nodes uh easily. Uh I would play with that then I would try to maybe wipe code this all these visualizations and then yeah we can then uh deploy them on the um some headner or some other place. Uh yeah and yeah just keep it running collect all the information then yeah upload it somewhere uh maybe O can help here because yeah he was also very yeah he was helping with all all these visualizations in the past. Uh yeah that's what I do. So what what I do now is like yeah would be really great if we had this if we introduce for example to Z all these CLI parameters. Um if you uh in parallel I would work on kind of running shadow simulations uh with maybe countries distribution so that yeah we allocate assign each node to some country and then yeah we uh yeah check uh like then we have more or less realistic distribution of latencies.
 
 
00:25:58
 
Kamil Salakhiev: Yeah put EIP7870 requirements. Uh I think you you said that you have problems with uh finalization even in a single subnet. Do you know what was the reason?
Parthasarathy Ramanujam: no not not with single subnet I'm saying that um
Kamil Salakhiev: Oh sorry.
Parthasarathy Ramanujam: uh I mean the aggregation time um when in multiple subnets is quite difficult or
Kamil Salakhiev: Okay. Why it is the case? You
Gajinder Singh: So I I I think what para is saying that the aggregator basically the aggregator computation itself needs to be optimized quite a lot or the machine specs needs to
Kamil Salakhiev: know,
Gajinder Singh: go up to basically accommodate a multi-ubnet aggregation because ze as such and I don't know other clients might also support uh being an aggregator of multiple subnets. So, so that that is what Partha was saying that once you basically start being an aggregator of more than one subnet uh since aggregator can't really handle
Kamil Salakhiev: Okay.
Gajinder Singh: it in a particular time window then basically the network obviously gets affected because payloads are not generated.
Kamil Salakhiev: Okay.
 
 
00:27:16
 
Kamil Salakhiev: But why are we so focused on being an aggregator in multiple subnets? Uh I think the default the default assumption is that yeah we have like yeah uh the aggregator is assigned just to a single subnet and if this is the case I mean Yeah, everything should be fine,
Gajinder Singh: I mean so uh so the benefit of an aggregator being in multiple subnets
Kamil Salakhiev: right?
Gajinder Singh: is that it can pro provide better payloads. So when for example the proposer is trying to bundle all those payloads together, it also again needs to run aggregation because it needs to aggregate all the payloads with the same attestation data because that is how our spec is and uh and and if you so if there are multiple payloads to be aggregated over time basically you are increasing the work on the proposer and if you are an aggregator over multiple subnets basically you are helping the proposer create a better payload so that it does not need to do that much work later because your number of payloads will obviously go by half for example let's say if you are an aggregator over two subnets so so I think we will also need to have a multiple subnet aggregators uh and there could actually be a super aggregator which is aggregating over all subnets if we sort of start to think like how PS And we can have a ps kind of a scenario in
 
 
00:28:42
 
Kamil Salakhiev: Yes, this is good.
Gajinder Singh: which we say that okay if so much stake is attached to you then you got to be an aggregator right uh and if that stake is again too high then you got to be aggregator over this this this particular subnet depending upon your node ID. So, so in that regard basically we need we will we will at a node level or a network level you can say like pas we will need to have some sort of a rule which will basically enforce that there are sufficient aggregators out there in the network uh doing the work so that if someone goes down right uh the block production does not stop. Lot production will not stop but basically the finalization or justification movement shouldn't
Kamil Salakhiev: Okay, we can um Yeah.
Gajinder Singh: Oh,
Kamil Salakhiev: So we yeah let's assume let's do maybe one step at a time. So yeah first uh I think the simpler assumption is that we have a single aggregator subnet I mean one aggregator is assigned to a single subnet. Yeah there might be multiple aggregators uh we can but anyway yeah we have shadow I mean we have I mean we can simulate whatever we want.
 
 
00:30:06
 
Kamil Salakhiev: Uh first like the default setup I would use is like yeah we have a single aggregator per subnet. Um and then and then yeah we kind of uh start playing with different assumptions. Um anyway everything will be in link quick start. So um it should be easy to configure. I mean we have this validators config file or how it's called uh like we put uh whatever uh flags and uh uh clients we want. Um yeah and then yeah we can simulate anything. Yeah, let's just um as I said like uh let's first as a next step uh yeah introduce uh different underlay topologies um that yeah I can do this um then if we want to have have a server uh yeah maybe introduce this visualizations um I mean I let me at least spend some maybe half a week uh uh trying to do that. If it's easy enough, then yeah, great. We can do this. If not, yeah, we just okay do simple simulations. Uh well, just just without nice visualizations, just yeah, collect logs and see how the station's propagated and so on.
 
 
00:31:30
 
Kamil Salakhiev: Um that's what I would do until maybe and we can get back on that uh for that on Wednesday. Um yeah we'll see how it goes and then yeah we can decide like okay whether we u whether we ready to have like a server which we can use to um uh start our runs I mean so that yeah everything kind of goes uh even when we I don't know just just some as I said like my I I my dream is to have some kind of art research so that yeah we have some AI even playing with different parameters then uh create some summaries and saying like hey we for example changed the mesh size to this size then we or we had this configuration when we have for example aggregator assigned to multiple subnets that was the result um yeah um on your
Gajinder Singh: So,
Kamil Salakhiev: side
Gajinder Singh: so we have so we have AI integration which looks into our lean quick start subnets and basically tries to debug it and probably we can also sort of direct it towards this particular thing like you're saying that okay it will analyze the results and basically suggest something on basis of that.
 
 
00:32:43
 
Kamil Salakhiev: Um ideally yes. Uh yeah I don't this is like big picture it's somewhere in future but that's that would be great to have at some point right yes I
Gajinder Singh: Future is
Kamil Salakhiev: mean in future I would say maybe uh let's just have some simulations first running
Gajinder Singh: now.
Kamil Salakhiev: uh smoothly uh so that we can configure aggregation time um we can configure latencies uh all of that when it when it works yeah I mean yeah then we can play with the start playing with
Gajinder Singh: Right.
Kamil Salakhiev: Okay.
Gajinder Singh: So on that on that front basically you have Kai as a dedicated uh person that can assist you in this followed up backed up by Partha and then by any of us right so uh basically we would like to offer our support to sort
Kamil Salakhiev: Right.
Gajinder Singh: of accelerate and help you in this so that even you know you are not really uh you basically the load on you is also less and uh most of the chunk of the work can be executed by
Kamil Salakhiev: Yeah. Yeah.
 
 
00:33:48
 
Kamil Salakhiev: Uh great. Yeah. From for now uh yeah. Yeah.
Gajinder Singh: Kai.
Kamil Salakhiev: I will just maybe keep posting sometimes uh maybe in Z channel or in PQ interrop channel like uh uh this is where we are like this is like link start branch which you can play take and play around uh as I already did uh like I think yesterday or today uh when I shared the branch um yeah I mean I can share some recommendations. So as I said uh being able to configure aggregation signature aggregation time start aggregation time uh signature verification time maybe uh these three things if they were if they are in zoom that would be great u uh so yeah and
Gajinder Singh: So, so Pat and Kai,
Kamil Salakhiev: then
Gajinder Singh: could you basically please make sure that whatever uh configuration camel requires, can we do a quick PR on this
Kamil Salakhiev: yes So we basically need a sleep time uh just putting artificial sleep in
Gajinder Singh: support?
Kamil Salakhiev: in zim just to be clear right so that say we have uh we say we put a signature aggregation rate to be thousand signatures per second and then uh yeah our aggregator received 60 signatures so that it means we need uh how much is that six millconds or 60 millconds 60 millconds uh of sleep uh to be put when we actually perform an aggregation in in the code u that's kind of the thing I I
 
 
00:35:16
 
Gajinder Singh: So, so basically this is this is the sleep we are saying to shadow is the time that we are
Kamil Salakhiev: expected.
Gajinder Singh: consuming Heat.
Kamil Salakhiev: Yeah. Yeah. It's just a sleep just I don't know how it's in zigg like in C++ std sleep for this amount of milliseconds and then yeah shadow uh because yeah as I said and and then yeah this
Gajinder Singh: Heat.
Kamil Salakhiev: would look like more realistic so that like like real aggregation happened because otherwise okay we we we do have real aggregation happening but as I explained in shadow simulator time it's zero milliseconds and here we're adding artificial sleep so that it's like 60 millconds which is closer to uh what we actually expect and because we don't know because the parameters uh for this uh uh well we we constantly evolving uh so we can play with different aggregation times. Yeah, that so that would be great. Ideally, ideally at some point this is not a requirement at all for now, but for some bright future uh would be great if we could also play for example with signature sizes with uh snark sizes so that um we can simulate for example like uh how the well how the network is going when we have this less than one MTU per signature.
 
 
00:36:34
 
Kamil Salakhiev: uh or when we have this narov size I don't know 70 kilobyte or so I think last time I spoke with he said that that might be even achievable at some point um anyway that's but yeah that's more comp complicated because then we we we do not really have today this kind of snarks and this kind of signature so uh they they would need to be artificial uh as well and this this really complicates the flow I I think we do have this in clean uh because yeah we just we were thinking about this from day one uh but yeah but it's also it introduces it's it's not easy to maintain I would say so don't bother with that anyway that's just my thoughts um okay good okay uh I think uh yeah just uh I'll work with uh different underlay topologies Uh yeah Kai if you can introduce these things to Zim so that yeah we have these artificial slips uh when we actually perform uh aggregation by default these sleeps they are just zero milliseconds so that they're not happening in production uh we only need them in shadow uh when we simulate shadow simulations um yes so that and then yeah we get back for that maybe on Wednesday and then yeah we see how it goes uh regards the next steps then yeah we can decide already like okay we do need a server because yeah we do have like a script and everything.
 
 
00:38:07
 
Kamil Salakhiev: Uh yeah that would be
Gajinder Singh: Yep,
Kamil Salakhiev: great.
Gajinder Singh: sounds like a plan. Thanks Camille for sort of leading this effort and helping us over there. And yeah,
Kamil Salakhiev: Yeah. Yeah.
Gajinder Singh: let's make it happen.
Kamil Salakhiev: Yeah. Good. Nice. Uh shadow channel interrupt.
Gajinder Singh: So feel free to leave if you want or you want to stay for the rest of the call. totally up to
Kamil Salakhiev: Yeah,
Gajinder Singh: you.
Kamil Salakhiev: I have some other stuff that I need to prepare for. So, yeah, have a good day, guys.
Gajinder Singh: All right,
Parthasarathy Ramanujam: Yes.
Gajinder Singh: thank you
Parthasarathy Ramanujam: Thank
Gajinder Singh: for all
Kamil Salakhiev: cuz I
Anshal Shukla: Tense.
Parthasarathy Ramanujam: you.
Gajinder Singh: right. So moving on to our aggregation and uh optimizations regarding that and DevNet for runs. So Bartha, what's going on over there?
Parthasarathy Ramanujam: Um so, uh still, uh I've been running this multi-ubnet devnet for the past week. Um uh every run has introduced a new issue that we have been looking into or addressing.
 
 
00:39:15
 
Parthasarathy Ramanujam: Uh initially after yesterday's run, I thought Ze's aggregation performance was much better than the others, but it uh um I mean we later on discovered that it was not actually so because we were missing quite a few uh aggregation slots. Uh so we are still there was an improvement that I uh applied yesterday and then Anel has a new PR today uh which would probably parallelize the aggregation we should keep an eye on that and see um I mean this is across the board not just with Zim everybody else is having the same issues um so unless we stabilize and achieve that aggregation within the given time slot I don't think we'd be achieving finality or justification on a mult multi- subnet uh devet. Uh so that should be our priority before uh we proceed to um I mean scale even further or introduce
Gajinder Singh: This this makes sense.
Parthasarathy Ramanujam: defet
Gajinder Singh: But why did we not catch this
Parthasarathy Ramanujam: uh I mean for various reasons.
Gajinder Singh: earlier?
Parthasarathy Ramanujam: First the issue was in the spec itself. respect did not mandate uh the need for uh I mean aggregating and probably we were missing some tests which we could probably add in the test suit and this is something I've asked u to as well uh we need better aggregation specific tests in the test suit and hive testing should also catch this because uh everything looks good probably in a two subnet devnet we nobody paid much attention to aggregations at that time but um it starts to show uh that it it's a problem when
 
 
00:40:56
 
Parthasarathy Ramanujam: we start scaling up.
Gajinder Singh: So what is the problem?
Parthasarathy Ramanujam: So uh I don't have a correct answer.
Gajinder Singh: You describe in TLDDR what the problem is and what you are trying to
Parthasarathy Ramanujam: So uh the problem is that our slot interval for aggregation
Gajinder Singh: solve.
Parthasarathy Ramanujam: is8 seconds. So an aggregator has to finish its aggregation and also finish the transmission of that aggregate within that8 uh second interval. But the problem here is that Ze uh takes probably on an average 3 seconds to complete the aggregation itself. Um one of the reasons was that we were uh doing recursive aggregation even though there was just one attestation available which shouldn't have happened. This was a bug in the spec itself. The second uh I believe which Anchel has discovered is that we were not parallelizing the aggregation. We were doing one aggregate at a time uh which was resulting in the delay. Once PR 915 is merged, we may have more uh I mean uh we'll know how it's actually working. Uh but this is uh a couple of things.
 
 
00:42:05
 
Parthasarathy Ramanujam: I initially thought maybe uh Zen uh was uh overloaded while processing the gossip but uh the current uh metrics show that there is no issue with the gossip. has received the attestations on time. It's just the aggregation process itself is taking longer. Um so uh that is something we'd have to
Anshal Shukla: uh I have just one doubt here.
Parthasarathy Ramanujam: do.
Gajinder Singh: Let's
Anshal Shukla: So you are saying that we perform well when there's a single subnet.
Gajinder Singh: go.
Anshal Shukla: So if aggregation is the bottleneck then how I'll be able to
Parthasarathy Ramanujam: Yeah.
Anshal Shukla: aggregate in a single subnet because single subnet in single subnet there are more childrens to aggregate and more gossip signatures to aggregate. So ideally we should work better with multiple
Parthasarathy Ramanujam: No, I I'll I'll tell you why.
Gajinder Singh: So I think what would be happening that on single this loss percentage would be
Parthasarathy Ramanujam: because
Anshal Shukla: subnets.
Parthasarathy Ramanujam: sorry
Gajinder Singh: same but spread over multiple subnets means that there is a greater loss. So something like that would be happening so that uh you are not crossing the two threshold anymore is assumption.
 
 
00:43:13
 
Parthasarathy Ramanujam: yeah the and the fork choice tree is completely different in a multi-ubnet each uh subnet has its own view of the chain um so when you um the folk tree in a single or a double subnet a DevNet was not as uh disjoint or spread across as you would see right now. Um so for example you see Zen uh um has multiple uh weighted for a couple of branches but none for the remaining. Each lambda has a similar view of its own chain. So each things its own chain is the the correct chain and misses on the others.
Anshal Shukla: Okay. So it gets to a point where uh aggregators have like multiple attestation datas because of the split in the folk choice and that is yeah makes
Gajinder Singh: I mean there should not be split in the fork choice.
Parthasarathy Ramanujam: Correct.
Anshal Shukla: sense.
Gajinder Singh: If they split in the fortress that means that even uh the proposers are not building on the same heads right.
Parthasarathy Ramanujam: So there is difference in the heads as well. So uh if I if you notice Zim 4 and Z8 uh the aggregators that Zim were part of were on different heads completely.
 
 
00:44:30
 
Parthasarathy Ramanujam: Ze was on 600 or Z 4 was on 600 Z 8 was on 512 ETH lambda was on 612 or something and RE was on 300 or something. So uh there was no uniform head across all of these chains. So uh something else is happening as well which uh I've not been able to isolate uh
Gajinder Singh: So the new metrics that I added were covering covering the subnet aggregation
Parthasarathy Ramanujam: yet
Gajinder Singh: uh subnet level uh coverage. So that should basically show you whether you are getting the aggregates from other subnets or not. So,
Parthasarathy Ramanujam: right so uh that was
Gajinder Singh: so what so what what is that data reflect
Parthasarathy Ramanujam: merged only last night I've not released uh devet for image yet.
Gajinder Singh: reflecting
Parthasarathy Ramanujam: I'll do that later uh today after this call and then uh provide you with an update.
Gajinder Singh: that was merged quite a quite not last night.
Parthasarathy Ramanujam: the estimate, the one that you said, right? You asked me to pick up
Anshal Shukla: No. So we already had those
 
 
00:45:34
 
Parthasarathy Ramanujam: uh
Gajinder Singh: So in that were merged on May lo atest stationation subnet coverage in
Anshal Shukla: metrics.
Gajinder Singh: chain status and then you did another PR of it which was about uh the
Parthasarathy Ramanujam: Okay, I'm
Gajinder Singh: gossip atestation coverage. So my PR was about aggregate uh subnet coverage and you also did a separate PR where you added a metric about uh gossip coverage. So so basically both of these things should give you a good idea of what aggregates as such you as a node are seeing and even the aggregator is seeing before you start aggregating. Right? So,
Parthasarathy Ramanujam: Yeah.
Gajinder Singh: so the metric should basically tell you what what the issue is in my opinion or
Parthasarathy Ramanujam: Okay. I'll I haven't tracked that yet.
Gajinder Singh: which particular you know which particular subnet you
Parthasarathy Ramanujam: I'll I'll do that. Yeah.
Gajinder Singh: are not actually seeing. So what the problem you're seeing is that you know you are not seeing a test stations from something right some particular subnet seems like that that is a
 
 
00:46:41
 
Parthasarathy Ramanujam: Good.
Gajinder Singh: problem and it basically becomes more in multiple subnets because then there are more fractured subsets that you are not seeing consistent data from and uh that that could be the problem. So not only us but other clients also need to add these metrics so that we can also see what other clients what aggregates other clients are seeing or what is the data that they are seeing that they are trying to aggregate
Anshal Shukla: So I think like the reason for this fracture can be that we aren't able to propose the block itself in time because of the compaction process that happens. Uh and I think I suggested it on Wednesday as well. I think we should lower down the maxist stationation data to something like four or eight. I think
Gajinder Singh: But but that is not that is not the problem of max resistation data, right?
Anshal Shukla: four.
Gajinder Singh: Your same restration data can for example have many payloads for that basically totally depends upon number of subnets you have. So if you have four subnets then even if everybody agrees then you will have four payloads to aggregate even for one at a station data for the
 
 
00:47:57
 
Anshal Shukla: Yeah. But in a block when I try to build a block I won't have to uh do the complexction
Gajinder Singh: proposal.
Anshal Shukla: process for uh 16 different attestation data. So I'll have like
Gajinder Singh: So, so what should what should happen is that there should be a boundary beyond which you should
Anshal Shukla: six
Gajinder Singh: stop bundling for other registration data. You you basically say that okay if you're greedily packing then and if your time boundary hits for a particular deadline then you should basically pack it and move on. Right? This is what you should do.
Anshal Shukla: Uh yeah that should that can be done but that is not how it is being done right now. Right now like the block is proposed and then the compaction is a different process and it will either do like a complete compaction or not and if like there are certain attestation data that cannot have like a complete compaction that can happen. So in that case we'll have to move few of the uh attestation data back to the latest payload latest aggregated payload map because after blog
 
 
00:49:12
 
Gajinder Singh: No, no, no.
Anshal Shukla: building you know I
Gajinder Singh: You don't move to the latest aggregated payload map. I mean latest you you already have the latest known,
Anshal Shukla: I think that
Gajinder Singh: right? So you just propose the block based on basis of whatever you have and uh uh basically it's up to the next proposer to fill in the gap because then the slots all slots will vote again right all validators will vote again on the slots. You don't have to remove anything back. You basically stop your process as soon as you hit a deadline and then basically you run your proposal with it. Otherwise you all anyway know that your proposal is going to
Anshal Shukla: Yeah.
Gajinder Singh: fail.
Anshal Shukla: Okay. Yeah. I think Okay. I think we can add this into the spec. I was also thinking in that direction that if we are we should have stopped building the block and for the number of max station data that we have already compacted we should just publish the block with them.
 
 
00:50:14
 
Anshal Shukla: I think that is what you mean by this. So I'll add this in the spec itself.
Gajinder Singh: Yes, correctly. Basically have a deadline and then run with it. I'm not sure this needs to be upstream to spec because these are client level optimizations and this is how there are how client these are this is how a production client differs from a spec. So basically making spec more complicated about this might not be a good idea. So this is just a client level optimization that we need to do unless you feel that we should upstream to spec then that is also fine. I'm not against that just that my idea is that spec should be kept simple but maybe you can uh discuss it with Toma if you think that this is something that should be done by all of the clients which I guess also makes sense then basically you can even upstream to spec as well if all the clients are seeing these problems but with respect to uh our aggregation Why is ETH lambda doing better than us
 
 
00:51:28
 
Parthasarathy Ramanujam: no uh yeah it's almost the same performance uh I mean but uh they
Gajinder Singh: still
Parthasarathy Ramanujam: see a lot lesser errors or skipped status aggregations than us uh which I believe might get resolved after the two PRs uh are merged. Unshel's parallelization PRN.
Gajinder Singh: and and the battle is so are we constructing some sort of a tree to recursive
Parthasarathy Ramanujam: Yeah.
Gajinder Singh: aggregate in the parallelization PR or what what is the kind of
Anshal Shukla: No.
Parthasarathy Ramanujam: Uh I
Gajinder Singh: fan out we are dealing with this over
Anshal Shukla: So,
Gajinder Singh: Yes.
Anshal Shukla: so right now uh so the PR that I just created uh it basically uh so what we were doing up till now was we were basically creating a new uh IO thread uh when when it was chance to aggregate and I think like that was like the wrong way to do it because we were creating a single IO thread and we were basically We had a concurrency limit there of one. So it was basically serial aggregation. I removed it and I have added like the thread pool uh thread pool aggregation how I did it in the uh compact requestations process.
 
 
00:52:45
 
Anshal Shukla: So uh so like there were multiple issues. One was that it was serially trying to serially aggregate and that was leading to the high number of inflight uh matrix that path had added and the number there was really high. uh apart from that like uh I think it doesn't make sense to spin up like a uh standard IO threaded thread out of attestation when in like the attest aggregation process because it is a CPUbound task. It is not an Ibound task. So uh not sure but I think like it doesn't make sense to have like a iOS specific thread because it basically specializes in uh doing like the concurrent work but we are it is not a concurren heavy uh
Gajinder Singh: So my my question is are you able to engage all the
Anshal Shukla: thing. Yeah.
Gajinder Singh: CPUs?
Anshal Shukla: So that is something that uh that I want uh access to so that I can basically do like a perf analysis and see how many cores are being uh uh utilized and what is like the percentage of cores that are being utilized.
 
 
00:53:56
 
Anshal Shukla: So I have asked Pada to give me access to the devet setup that he runs so that I can basically monitor the do like use perf tool to basically see how much percentage of the core is being occupied and uh basically then optimize it on
Parthasarathy Ramanujam: What's
Gajinder Singh: So reason to have more IO threads than what we think is the best optimal
Anshal Shukla: our
Parthasarathy Ramanujam: so
Gajinder Singh: is that we leave it to operating system to manage uh scheduling between them. Right? So in that sense it does not hurt. I mean don't try to overoptimize in terms of less number of threads that we want to keep. basically just say that okay if we have X CPUs then we will have X threads doing the compute even if each of the compute is uh uh sort of CPU parallelized or multi-core compute right because we don't know what actually uh how the underlying library how how efficient is it is on multi- CPU multi-core compute so so those are all the things so don't basically you know try to overoptimize and try to restrict yourself with regard to uh the threading uh just have trust that operating system will do the right thing and
 
 
00:55:18
 
Anshal Shukla: Got it. Also like there's another thing
Gajinder Singh: and on the aggregation topology what what is the best aggregation topology do you are you forming a
Anshal Shukla: we
Gajinder Singh: tree to aggregate or so so what is the aggregation topology that you are following and the second thing is does aggregation topology even matter right or you basically say that okay I am just taking out uh five five aggregates and you know building a tree with it or I'm just I have a target aggregate in which each time I have five new payloads I aggregate into it so what is the topology that you are following for the aggregation and what is the best topology to go for over
Anshal Shukla: So there's no topology as such because it there's no max we do like a greedy selection of children and based on that we do like In a single run the API itself provides us with an option that in a single run we can do like the complete aggregation. So there's just the greedy selection that happens and post that we do the aggregation.
 
 
00:56:28
 
Gajinder Singh: Okay. So basically you are leaving it to the underlying library to
Anshal Shukla: Yeah.
Gajinder Singh: construct a tree or whatever they want right that is fine and it's also a good approach.
Anshal Shukla: Yeah.
Gajinder Singh: Uh so because it is their job to maximize to optimize this sort of uh aggregation structure if they benefit
Anshal Shukla: Yeah.
Gajinder Singh: out of it.
Anshal Shukla: Yeah. So I already had this discussion with email earlier about uh optimizing it in a treebased format but uh I don't know how he implemented it eventually.
Gajinder Singh: Right. So I mean just even forgetting about that my question is then why why do we need
Anshal Shukla: So,
Gajinder Singh: multiple threads slash multiple processes of aggregation running over there because are there so many adestration datas?
Anshal Shukla: uh, there can
Gajinder Singh: How manyestration datas are there and is that in uh uh is that available in some sort of a metric as well?
Parthasarathy Ramanujam: So as far as I know um the uh lean multisc library by itself parallelizes the aggregation. So uh there is no specific
 
 
00:57:52
 
Gajinder Singh: Yeah, I'm not saying that. I'm basically I'm not saying that.
Parthasarathy Ramanujam: metric.
Gajinder Singh: I'm saying
Anshal Shukla: Yes. So there are definitely multiple attestation data because that's why we have that uh inflight uh
Gajinder Singh: that
Anshal Shukla: matrix which shows a huge number.
Parthasarathy Ramanujam: Yeah.
Anshal Shukla: Had there been like just one uh then in that case we won't have seen such a large number of
Parthasarathy Ramanujam: Yeah.
Anshal Shukla: inflight uh inflight attestations which have basically led
Gajinder Singh: So, so the highest priority should be given to attestation data that match locally with your local view,
Anshal Shukla: to
Gajinder Singh: right? So, you should basically schedule that first and publish that first. In fact, don't even wait for everything to uh uh to basically, you know, compact and publish, right? So don't wait for other aggregations to happen. So if you if something is matching with your local view that is best destination data for you right you should aggregate that first and publish that first given that has enough support uh or you basically pick other things and if you have CPU cores left cores basically then you you could assuming that each of these computation is all already multi-threaded then I think it is better that it be a serial process because then you would want to give your full CPU power to the best stationation data that you think.
 
 
00:59:20
 
Gajinder Singh: So, so you have a sequence in which you can order your test station datas and say that okay I will process this send this out I'll pro then I'll process this and send this out. So, so instead of waiting for everything to get completed, I think this kind of priority processing as well as transmission is required. So uh what the best thing is again whatever you that you have made basically provide a CLI param to adjust that parallelization factor right and if we put it on one that basically means that it will process the in in basically in a particular order uh based on the priority that we that the local node evaluates that this is this looks like the best destination data. So I will compact it first and then I'll transmit it and if for example there is uh concurrency factories too then basically it will take first two and process them and but it will not wait for both of the two to get completed to publish whichever gets done you just publish it right away.
Anshal Shukla: Okay.
 
 
01:00:54
 
Gajinder Singh: So let's do this follow PR I mean and still keep your polarization work uh in 191 or whatever that is. So I'll also take a cursory look at it but you can take a deeper look at it.
Parthasarathy Ramanujam: Yeah.
Gajinder Singh: Anything else we can do? So do we do we have do we also have some time difference
Parthasarathy Ramanujam: Heat.
Gajinder Singh: where you see the test stations spread out over time because then you can probably start early right you can instead of waiting to start the compaction on the new interval you can probably start compaction as soon as you start seeing two or three or whatever variable we have defined para for compaction to start.
Parthasarathy Ramanujam: Okay.
Gajinder Singh: So this is another thing that we can start
Parthasarathy Ramanujam: All right. Uh, one other thing I noticed uh in our logs where it says the head slot,
Gajinder Singh: doing.
Parthasarathy Ramanujam: the current slot and behind. What does that behind imply? Is that does that mean our uh uh client is asset slow or uh processing late?
 
 
01:02:16
 
Gajinder Singh: Instead of behind, rename it to missed. You could have missed because there was no proposal or you could have missed because you are behind actually. So, so there there is no way no way to know unless you are inspecting the client and you know that okay you are
Parthasarathy Ramanujam: Okay.
Gajinder Singh: not behind you didn't get any I guess you can you can say if if there is nothing in the
Parthasarathy Ramanujam: Okay. because in few cases uh right
Gajinder Singh: queue then you are not behind you have missed it.
Parthasarathy Ramanujam: okay because I on occasionally I've seen that Ze when it's running as an aggregator is behind 24 slots uh and then it suddenly catches up uh to zero. So I I wasn't quite sure how that could
Gajinder Singh: So that that means that that means that either that particular z node or some other z
Parthasarathy Ramanujam: happen.
Gajinder Singh: node produced a new block that that that's that's whose parent was 24 slots behind. That is the easiest way I can explain. why this could happen or your ze node did not see 24 slots in
 
 
01:03:17
 
Parthasarathy Ramanujam: Okay.
Gajinder Singh: between and then somebody proposed right.
Parthasarathy Ramanujam: Right.
Gajinder Singh: So instead of behind I guess you can say mist and now mist might
Parthasarathy Ramanujam: Okay.
Gajinder Singh: actually mean missed or orphaned or whatever right whatever you want to call that terminology but yes behind might give a negative connotation in the sense that the node is
Parthasarathy Ramanujam: Okay,
Gajinder Singh: slower when even when there is misproposals or something else has happened in the network for example a block was not correct so you rejected it so
Parthasarathy Ramanujam: there.
Gajinder Singh: let's Change behind to missed I guess.
Parthasarathy Ramanujam: Okay.
Gajinder Singh: So anything else we have on the plate? Kai, you have any ideas of improving the aggregator performance?
Kai Chen: Um no I just uh I just noticed um aggregation already parallel in ROS side. So so I'm not uh so I think you said uh you idea maybe uh maybe good. So I currently I have no uh other ideas but
Gajinder Singh: Got it.
Kai Chen: that
Gajinder Singh: Just everybody keep in mind how we can you know just apply your brain cycles on it.
 
 
01:05:05
 
Gajinder Singh: How we can further improve it. Throw ideas. No idea is a bad idea. We might some we might get a good idea out of it. So just keep thinking on it and let's make the aggregation bottleneck you know. Let's solve
Anshal Shukla: There's another thing that I was looking into and probably I'll raise a PR.
Gajinder Singh: it.
Anshal Shukla: It is not related to aggregation but right now in network and networking layer and the DB layer, we don't use the async IO uh syntax that was recently reintroduced back in Z.16. So I'll reintroduce back and we'll have some concurrency there.
Gajinder Singh: Yep. Sounds good. And I I think it will also sort of make code cleaner. So that is anyway a win over longer time
Kai Chen: Um so but I think it's in zero do uh
Gajinder Singh: frame.
Kai Chen: 0.16 the the async IO is is not ready in Z. So right now there the there uh currently the there is only multi thread I
Anshal Shukla: Uh, okay.
 
 
01:06:28
 
Anshal Shukla: I'll look into that as well. I don't exactly
Kai Chen: yeah so the event event IO is is not ready in this
Anshal Shukla: understand.
Kai Chen: version. But but I found Yeah. But I found another library uh which which
Gajinder Singh: So
Kai Chen: name CIO. So I think we can use it. So, and also we can uh and and also I think we can use it to instead of the LE XCV because because the Liu XCV is is not uh compar comparable with the 0.16 upgrade mentioned in in his uh uh latest uh latest PRs I think so so it's it I think the the the GIO library maybe is the best options we use
Gajinder Singh: Yep. So I'm sure it makes sense to yeah look into what uh Kai is suggesting regarding the new library and uh yeah I think Leave CV was a hack. So, let's move to something better. Nou, do you have anything for us?
Noopur Singh: Um I I'm just posting the uh rolling actions from the team meetings.
 
 
01:08:27
 
Noopur Singh: Uh so you can take a look. Other than that uh not really. I'll work on the CI uh this weekend for the shadow. Uh and that's it.
Gajinder Singh: All
Noopur Singh: So this rolling actions was created by Zclaw Zclaw and uh they it
Gajinder Singh: right.
Noopur Singh: has all the u like the Z uh groups chats uh and uh and the uh Z meeting transcripts. So it has accumulated all three places um and then created this
Kai Chen: Oops.
Noopur Singh: rolling action.
Gajinder Singh: So can we also sort of do an automated posting of this after every
Noopur Singh: So
Gajinder Singh: call in the sense that it automatically gets posted in the group.
Noopur Singh: okay. Okay.
Gajinder Singh: See if we can do
Kai Chen: Holy s***.
Noopur Singh: So for uh for that I need uh like the for the transcript
Gajinder Singh: that.
Noopur Singh: uh posting that you asked that transcript automation that it should be public. So for that I would need to uh coordinate with you because uh the because by default the uh the Zen the recordings the Google meet recordings are not uh public and they cannot be made public.
 
 
01:09:46
 
Noopur Singh: So we'll have to transfer them uh I'll have to write a script on in Google script so that
Gajinder Singh: Okay.
Noopur Singh: which will transfer the uh the transcript to a public uh folder.
Gajinder Singh: Or you can have some sort of a integration where basically you know it does auto transcription and joins in the call right so there are tools like that if those tools are handier than this particular
Noopur Singh: Okay. Yes.
Parthasarathy Ramanujam: Uh can we have this on a separate channel because it can uh we may lose if you you want to
Gajinder Singh: Okay.
Noopur Singh: Okay.
Parthasarathy Ramanujam: refer uh to the transcript or actions maybe
Kai Chen: Oops.
Gajinder Singh: Yeah, we can create uh some sort of a separate channel on a topic. Do you mean topic or channel?
Parthasarathy Ramanujam: yeah topic topic sorry uh I'm talking in slack terminology I'm yeah I was referring to a separate topic on
Noopur Singh: Okay.
Gajinder Singh: Great.
Noopur Singh: Okay, I'll do that.
Gajinder Singh: All right. I we are over time and uh unless anyone
 
 
01:10:59
 
Parthasarathy Ramanujam: So uh sorry Gajendra before we go just wanted to check.
Gajinder Singh: Yeah.
Parthasarathy Ramanujam: So we wait until Caramel comes back before we place an official request for a machine right for shadow. Uh he uh appears to
Gajinder Singh: Yes. But uh but we start looking into the sleep configuration that he wants us to do,
Parthasarathy Ramanujam: be
Gajinder Singh: right?
Parthasarathy Ramanujam: Yeah. Yeah. that that that we'll do.
Gajinder Singh: So
Parthasarathy Ramanujam: I'm talking about the actual uh budgeting of the server and other requirements.
Gajinder Singh: yes,
Parthasarathy Ramanujam: We wait and it comes up.
Gajinder Singh: it's let's wait for wait for PM to come back because he has a 46 48 GB
Parthasarathy Ramanujam: Okay.
Kai Chen: The
Gajinder Singh: machine. So why
Parthasarathy Ramanujam: Okay.
Kai Chen: other
Gajinder Singh: not?
Parthasarathy Ramanujam: But your your idea for the end of the year was how many nodes uh or rather how many validators you want to test in um the plan. What do you think can be No,
Gajinder Singh: I I don't we'll figure it out later.
 
 
01:11:46
 
Parthasarathy Ramanujam: no, that's fine.
Gajinder Singh: It is
Parthasarathy Ramanujam: I'm just trying to understand what would be uh a convincing number for us to probably convince the
Gajinder Singh: a
Parthasarathy Ramanujam: rest of the uh codevs to look into this seriously.
Gajinder Singh: I mean uh how many validators do we have on hoodie
Parthasarathy Ramanujam: I don't know.
Gajinder Singh: that if I guess if we
Parthasarathy Ramanujam: Oh, okay. To match that
Gajinder Singh: match those not the uh not the validators that external people are running the hoodie validators that uh each pandops is running I guess or we we might not even match that we might even go a bit lower from it because our computation requirements are more are higher because of the new crypto and uh so
Parthasarathy Ramanujam: Good night. Mhm.
Gajinder Singh: in that regard I mean we'll we'll see basically but yes some sort of a fraction of that not a small fraction but some reasonable fraction of that could
Parthasarathy Ramanujam: Okay. Okay. Sounds good.
Gajinder Singh: All right guys, uh thank you for being in this call and see you on Wednesday in the PK interrupt call 26 oh 27th and then on 29th for the normal zine call.
Parthasarathy Ramanujam: Yep.
Anshal Shukla: Thanks. Bye-bye.
Gajinder Singh: Thank you.
Kai Chen: Thank you.
Parthasarathy Ramanujam: Speak to B.
Noopur Singh: Thank you.
Gajinder Singh: Bye.
 
 
Transcription ended after 01:13:14

This editable transcript was computer generated and might contain errors. People can also change the text after it was created.
