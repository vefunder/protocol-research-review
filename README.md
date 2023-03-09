# Protocol Research Review

This repository compiles a list of peer-reviewed research on topics reviewed by the Cryptorisks Team. The flow is as follows:\

1. Contributors must first go to the veFunder DeWork Job board to look for bounties. The job board can be accessed via: [https://app.dework.xyz/vefunder-1/main-space-95](https://app.dework.xyz/vefunder-1/main-space-95).
2. Apply for a job under to `To Do` list and await for confirmation from one of the admins. Please add relevant resume: articles, code, research, and projects that demonstrate the ability to conduct research that is helpful in answering questions posed in the Task.
3. Once you've been chosen, your task bears your name and goes to the `In Progress` tab.
4. Your research submission is reviewed via a pull-request. Your task goes under the `In Review` tab.
5. After a satisfactory review, your research is published at the Cryptorisks Substack: [https://cryptorisks.substack.com/](https://cryptorisks.substack.com/), following by a payout of the proposed bounty.

# Contribution

### Forking repo and writing research:

To put forth your research article for review, please follow the steps outlined below:

1. Install git: [https://github.com/git-guides/install-git](https://github.com/git-guides/install-git)
2. Fork the Protocol Research Review repository (top right of this repo): [https://docs.github.com/en/get-started/quickstart/fork-a-repo](https://docs.github.com/en/get-started/quickstart/fork-a-repo)
3. Clone your forked repo via:

```
> git clone https://github.com/<your-github-username>/protocol-research-review.git
```

4. Write your article as a markdown file under `articles > PROTOCOL_NAME > your_research > your_article.md` and push it to your remote origin via:

```
> git add articles/<PROTOCOL_NAME>/your_research
> git commit -m "<some commit>"
> git push
```

## Submitting research for review:

Research will be reviewed via a pull request. To do this, [please follow instructions provided here.](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/creating-a-pull-request-from-a-fork)


## Updating your local copy of the forked repo:

Should you ever need to pull any changes in the upstream repository, always `rebase`:

```
> git remote add upstream https://github.com/vefunder/protocol-research-review.git
> git fetch upstream
> git rebase upstream/main
> git push origin main --force
```

# Editorial standards

(Work in progress ...)

1. Do not plagiarise!
2. Use grammarly before submissions!
3. Provide appropriate citations.
4. If research involves code, pleae add working code to your submission (no data! that would bloat the repo).
5. Try to follow the [template](https://github.com/vefunder/protocol-research-review/blob/main/admin/templates/Report_template.md)

## House style

* Use "it's" rather than "it is"
* Use "end user" rather than "enduser"
* Use "multisig" rather than "multi-sig"
* Use the Oxford comma 
* Use "blue chip" rather than "bluechip"
* American English spellings (e.g. "centralize" rather than "centralise")

When in doubt, please follow the template used in the articles in the [Cryptorisks substack](https://cryptorisks.substack.com/).

Good luck! It is thanks to your efforts that we can self-police and decentralise our economy.


