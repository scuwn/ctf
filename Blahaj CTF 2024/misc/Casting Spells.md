# Casting Spells

### Problem
<insert pic of the netcat>

We are given a SSH fingerprint and its ASCII art, and asked to "send a counter colliding spell", where we need to send a different fingerprint that generates the same ASCII art (collision!).


### Solution
[The drunken bishop: An analysis of the OpenSSH fingerprint visualization algorithm](http://www.dirk-loss.de/sshvis/drunken_bishop.pdf)
- Explains in detail how the ASCII art is generated


### Solve Script