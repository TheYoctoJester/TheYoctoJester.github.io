# Live Coding Session #15 - Running applications

When: Thursday August 27th, 2020 - 5:00 PM Europe/Berlin

Topic: Running applications in the context of a yocto-generated distribution, with focus on systemd.

## Notes from session

JPEW: the init system can also be set through the [INIT_MANAGER](https://www.yoctoproject.org/docs/3.1/ref-manual/ref-manual.html#migration-3.0-init-system-selection) variable

Generally, this session and layer try to adhere to real life best pracises instead of quick demos:

- a distinct DISTRO configuration is being used
- a custom image for the mentioned application is created

## Session

### Recording at

[Youtube](https://youtu.be/9jMu8VNKSvU)

### LinkedIn Event

[Yocto Livecoding - Running applications](https://www.linkedin.com/events/yoctolivecoding-runningapplications)

### OE Calendar

[Yocto Livecoding Session 15](https://calendar.google.com/event?action=TEMPLATE&tmeid=NHFybDJyanY5cG1lMTc4N3RsYzY5ZHRyNHEgZ3N1Nm01Zzl1dGw0ZWxramxjdDE0NGloa29AZw&tmsrc=gsu6m5g9utl4elkjlct144ihko%40group.calendar.google.com)

## External resources

[meta-quicksilver](https://github.com/TheYoctoJester/meta-quicksilver.git): the layer created during the sessions

[simple-echo-server](https://github.com/TheYoctoJester/simple_echo_server.git): a simple tcp echo application for demonstration puposes, which can be tested with telnet.