# SANCTUS

Signals for Aid Near Conflicts To Unify Support 

**Status:** In Development
**Phase:** Specification

## What Is SANCTUS?

SANCTUS is an open framework designed to improve humanitarian response efforts by reducing needless casualties in conflict zones caused by the "fog of war".

## How Does SANCTUS Work?

The core of SANCTUS is the SPIRITUS beacon protocol. SPIRITUS beacons are radio transmitters that function similar to the ADSB transponders in an aircraft. These portable beacons are designed to transmit identity & location data humanitarian & aid workers to military personnel on all sides of a conflict to ensure that aid workers are not mistakenly identified as combatants.

To secure these beacons and prevent them being used for subterfuge/perfidy, each organization has its' own PGP encryption key, and each beacon they issue to staff will have its' own PGP key that is signed by the organization. Organizations can then share their "root" public key with governments to store in their own receivers. These keys can then be used to authenticate a beacon as belonging to an aid organization.

Governments will then issue appropriate receivers to their troops which will also store a copy of the database holding the root public keys for organizations in their area of operations. This will enable troops to quickly identify and locate aid workers and non-combatants, ensuring that these individuals are not improperly targeted, which is increasingly important in the era of drone warfare.

Due to the decentralized nature of this system, the one-way nature of the communication protocol, and the independent & open nature of the standard, this system may be implemented by any nation in the world without compromising their own strategic security. In short, all one needs to do is listen.

## How Can I Contribute

One of the most important aspects of developing this framework is good communication among all stakeholders who are invested in this. For individuals, joining our [discussion board](https://github.com/NoahGWood/Sanctus/discussions) & posting your thoughts/ideas on how to improve the framework will be very helpful. 

Some other ways you can get involved are:

* **Raise Awareness**
  Share the SANCTUS framework with friends & colleagues! Getting the word out is one of the best ways to help make SANCTUS a reality.
* **Write Your Government**
  If you can, try writing to your [Congressman/woman](https://www.house.gov/representatives/find-your-representative) about this project. U.S. Citizens provide a large portion of humanitarian aid, and instituting systems to protect them should be a national priority.
* **Translation & Localization**
  If you know another language, translating these specifications will be invaluable to helping everyone adopt this framework.
* **Technical Contributions**
  Individuals with expertise in software, cryptography, radio communication, or related fields can contribute by developing SANCTUS protocols, algorithms, and implementations.
