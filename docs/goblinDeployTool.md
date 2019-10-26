# An overview of Goblin's unnamed deploy tool
To power Goblin Cloud we use a proprietary deploy tool that provides us a fully container-less deployment and cluster orchestration. Moreover, it's fully Node.js based & without native dependencies, describes all DevOps processes on plain javascript. Here an example:
```javascript
var digitalOcean = require('DigitalOcean'),
    utils = require('./utils.js');

var newHostName = `${args.project}-${args.env}-node`;

if(digitalOcean.filterDroplets(e => e.name === newHostName).length){
    exit(new Error(`Already got host with name ${newHostName}`));
}

var host, buildTry = 20;

for(let i = buildTry ; i >= 0 ; i--){
    try{
        host = digitalOcean.createDroplet({
            name: newHostName,
            region: digitalOcean.REGION.FRA1,
            size: digitalOcean.SIZE.S_1CPU_1GB,
            image: digitalOcean.OS.UBUNTU_16_04_X64,
            ipv6: true,
            monitoring: true,
            tags: [args.project, args.env, 'node', 'automatically-generated'],
            private_networking: true,
            backups: false
        });
    } catch(err){
        exit(err);
    }

    if(utils.checkAccessibility(host, [80, 443])){
        break;
    } else if(i === 0){
        exit(new Error(`Failed to build new host - all addresses banned`));
    } else {
        host.destroy();
    }
}

host.resize({
    size: digitalOcean.SIZE.S_2CPU_4GB,
    disk: true
});

utils.tuneSysctl(host);

host.cmd('apt-get update && apt-get upgrade').run({ answerY: true, waitInterSec: 120 });
host.cmd('export DEBIAN_FRONTEND=noninteractive').run();
host.cmd('apt-get install mc unzip git').run({ answerY: true, mandatory: ['mc', 'unzip', 'git'] });

exit({
    name: newHostName,
    host: host.networks.v4.find(e => e.type === 'public').ip_address,
    host6: host.networks.v6[0].ip_address,
    privhost: host.networks.v4.find(e => e.type === 'private').ip_address
});
```

Note that there is no single `callback`, `promise` or `async/await` - all code synchronous. We use a special trick to fully get rid of any asynchronous functions - we believe that it helps people that have javascript only as a minor language to work with this tool in comfort.

It covers many DevOps needs like deployment of everything, config delivery, scaling, tuning, data migration, updating and managing artifacts, stoping/running/restarting apps, managing automated benchmark processes, sending emails and many more. Right now this tool is closed source and feeds only our cloud but we making all efforts to make it open source.