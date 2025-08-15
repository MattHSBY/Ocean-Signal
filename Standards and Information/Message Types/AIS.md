The AIS (Automatic Identification System) is an automatic tracking system that uses transceivers on ships and is used by vessel traffic services (VTS). The [[MOB2]] AIS operates on the [[VHF]] band. Shortly after activation an AIS location device, such as the [[MOB2]], will activate an MOB target and message on plotters in all AIS equipped vessels within the [[VHF]] range alerting them to the fact that emergency assistance is required. Often it is a vessel in the close vicinity of an incident that is able to react and effect a rescue quicker than the emergency services. 
The AIS transceiver will send data in bursts of 8 messages once per minute. Although it sends 8 individual messages during each minute, each burst sends 2 of each designated message, one message for AIS channel 1, and the other message for AIS channel 2, so it is effectively 4 messages but duplicated on 2 channels.
### [[MOB2]] Specification for AIS
The transmission format is as follows for the first and fifth bursts (minute 1 and minute 5).
* AIS 1, Message 1, Nav Status = 14, comm-state (time-out={7,3}, sub-message=0)
* AIS 2, Message 1, Nav Status = 14, comm-state (time-out={7,3}, sub-message=0)
* AIS 1, Message 1, Nav Status = 14, comm-state (time-out={7,3}, sub-message=0)
* AIS 2, Message 1, Nav Status = 14, comm-state (time-out={7,3}, sub-message=0)
* AIS 1, Message 14 "MOB ACTIVE"
* AIS 2, Message 14 "MOB ACTIVE"
* AIS 1, Message 1, Nav Status = 14, comm-state (time-out={7,3}, sub-message=0)
* AIS 2, Message 1, Nav Status = 14, comm-state (time-out={7,3}, sub-message=0)

The transmission format is as follows for the second, fourth, and sixth burst (minute 2, minute 4 and minute 6).
* AIS 1, Message 1, Nav Status =14, comm-state (time-out={6,4,2}, sub-message=slot)
* AIS 2, Message 1, Nav Status =14, comm-state (time-out={6,4,2}, sub-message=slot)
* AIS 1, Message 1, Nav Status =14, comm-state (time-out={6,4,2}, sub-message=slot)
* AIS 2, Message 1, Nav Status =14, comm-state (time-out={6,4,2}, sub-message=slot)
* AIS 1, Message 1, Nav Status =14, comm-state (time-out={6,4,2}, sub-message=slot)
* AIS 2, Message 1, Nav Status =14, comm-state (time-out={6,4,2}, sub-message=slot)
* AIS 1, Message 1, Nav Status =14, comm-state (time-out={6,4,2}, sub-message=slot)
* AIS 2, Message 1, Nav Status =14, comm-state (time-out={6,4,2}, sub-message=slot)

The transmission format is as follows for the third burst (minute 3).
* AIS 1, Message 1, Nav Status = 14, comm-state (time-out=5, sub-message=0)
* AIS 2, Message 1, Nav Status = 14, comm-state (time-out=5, sub-message=0)
* AIS 1, Message 1, Nav Status = 14, comm-state (time-out=5, sub-message=0)
* AIS 2, Message 1, Nav Status = 14, comm-state (time-out=5, sub-message=0)
* AIS 1, Message 1, Nav Status = 14, comm-state (time-out=5, sub-message=0)
* AIS 2, Message 1, Nav Status = 14, comm-state (time-out=5, sub-message=0)
* AIS 1, Message 1, Nav Status = 14, comm-state (time-out=5, sub-message=0)
* AIS 2, Message 1, Nav Status = 14, comm-state (time-out=5, sub-message=0)

The transmission format is as follows for the seventh burst (minute 7).
* AIS 1, Message 1, Nav Status = 14, comm-state (time-out=1, sub-message=utc)
* AIS 2, Message 1, Nav Status = 14, comm-state (time-out=1, sub-message=utc)
* AIS 1, Message 1, Nav Status = 14, comm-state (time-out=1, sub-message=utc)
* AIS 2, Message 1, Nav Status = 14, comm-state (time-out=1, sub-message=utc)
* AIS 1, Message 1, Nav Status = 14, comm-state (time-out=1, sub-message=utc)
* AIS 2, Message 1, Nav Status = 14, comm-state (time-out=1, sub-message=utc)
* AIS 1, Message 1, Nav Status = 14, comm-state (time-out=1, sub-message=utc)
* AIS 2, Message 1, Nav Status = 14, comm-state (time-out=1, sub-message=utc)

The transmission format is as follows for the eighth burst (minute 8).
* AIS 1, Message 1, Nav Status = 14, comm-state (time-out=0, sub-message=incr)
* AIS 2, Message 1, Nav Status = 14, comm-state (time-out=0, sub-message=incr)
* AIS 1, Message 1, Nav Status = 14, comm-state (time-out=0, sub-message=incr)
* AIS 2, Message 1, Nav Status = 14, comm-state (time-out=0, sub-message=incr)
* AIS 1, Message 1, Nav Status = 14, comm-state (time-out=0, sub-message=incr)
* AIS 2, Message 1, Nav Status = 14, comm-state (time-out=0, sub-message=incr)
* AIS 1, Message 1, Nav Status = 14, comm-state (time-out=0, sub-message=incr)
* AIS 2, Message 1, Nav Status = 14, comm-state (time-out=0, sub-message=incr)