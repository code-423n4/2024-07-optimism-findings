# L-01 If `if challengeDuration == MAX_CLOCK_DURATION` it will still revert

-----


Inside `getChallengerDuration()`, the following check is made:
```javascript
    function getChallengerDuration(uint256 _claimIndex) public view returns (Duration duration_) {
        // INVARIANT: The game must be in progress to query the remaining time to respond to a given claim.
        if (status != GameStatus.IN_PROGRESS) {
            revert GameNotInProgress();
        }

        // Fetch the subgame root claim.
        ClaimData storage subgameRootClaim = claimData[_claimIndex];

        // Fetch the parent of the subgame root's clock, if it exists.
        Clock parentClock;
        if (subgameRootClaim.parentIndex != type(uint32).max) {
            parentClock = claimData[subgameRootClaim.parentIndex].clock;
        }

        // Compute the duration elapsed of the potential challenger's clock.
        uint64 challengeDuration =
            uint64(parentClock.duration().raw() + (block.timestamp - subgameRootClaim.clock.timestamp().raw()));
->      duration_ = challengeDuration > MAX_CLOCK_DURATION.raw() ? MAX_CLOCK_DURATION : Duration.wrap(challengeDuration);
    }
```
Let's take for example the following values:
- `challengeDuration = 100`
- `MAX_CLOCK_DURATION.raw() = 100`

Since `challengeDuration IS NOT BIGGER THAN MAX_CLOCK_DURATION.raw()` -> Duration.wrap(challengeDuration), which is just `100`.

Now, this getChallengerDuration() function is used inside of move to get the duration:
```javascript
        Duration nextDuration = getChallengerDuration(_challengeIndex);

        // INVARIANT: A move can never be made once its clock has exceeded `MAX_CLOCK_DURATION`
        //            seconds of time.
->      if (nextDuration.raw() == MAX_CLOCK_DURATION.raw()) revert ClockTimeExceeded();
```

The problem is that it will still fail this check inside of move, even though it passed the check inside `getChallengeDuration()`

## Recommended Mitigation steps
Make both conditional checks test the equal scenario.