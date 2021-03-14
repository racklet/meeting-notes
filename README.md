# meeting-notes

This is a repo where we store public community meeting notes from the Racklet project.

Meeting notes are mainly edited using [`hackmd.io`](https://hackmd.io) online, in real-time,
during meetings. Everyone is welcome to contribute to the agenda of the meetings. The meeting
notes for respective meeting type is found as sub-folders in this repo.

**Note:** You do not need any authentication to edit the documents in HackMD, in other words, they
function more or less as a Google Document that is world-editable, and snapshotted after each meeting.

The notes are edited using HackMD in real-time using their editor. HackMD's supported syntax
seems to be more or less a superset of what GitHub supports, which means the documents might
not render as nicely in GitHub as in HackMD. After the meeting, the maintainer (which has push
access to this repo) should perform the following steps to keep HackMD and GitHub in sync
(to essentially snapshot a set of the meeting notes into this aggregate repository):

1. Make sure you have signed into HackMD, and allowed it OAuth2 access to your GitHub account, and have push access to this repo.
2. Click the three dots `...` in the upper-left corner, and select `Versions and GitHub Sync`
   1. <img src="docs/images/2.png" width="400"/>
3. You will now see a diff since the last version that was pushed.
   1. **Note:** To avoid unnecessary complexity, the flow is one-way only; HackMD should always push its changes to GitHub, and not vice versa, meaning these files should NOT be edited "by hand" in GitHub usually.
   2. <img src="docs/images/3.png" width="400"/>
4. Click the "Push" button.
5. The branch should be filled in automatically (if not, set it to `main`). Describe the changes shortly as follows, this will be the commit message.
   1. <img src="docs/images/5.png" width="400"/>
6. Confirm the push operation by clicking the blue `Push` button
7. Now the change will show up in the GitHub UI as follows:
   1. ![GitHub commit message](docs/images/7_1.png)
   2. ![GitHub commit diff](docs/images/7_2.png)
8. Cool, you're done :)

## Contributing

Please see [CONTRIBUTING.md](CONTRIBUTING.md) and our [Code Of Conduct](CODE_OF_CONDUCT.md).

Other interesting resources include:

- [The issue tracker](https://github.com/racklet/racklet/issues)
- [The discussions forum](https://github.com/racklet/racklet/discussions)
- [The list of milestones](https://github.com/racklet/racklet/milestones)
- [The roadmap](https://github.com/orgs/racklet/projects/1)
- [The changelog](https://github.com/racklet/racklet/blob/main/CHANGELOG.md)

## Getting Help

If you have any questions about, feedback for or problems with Racklet:

- Invite yourself to the [Open Source Firmware Slack](https://slack.osfw.dev/).
- Ask a question on the [#racklet](https://osfw.slack.com/messages/racklet/) slack channel.
- Ask a question on the [discussions forum](https://github.com/racklet/racklet/discussions).
- [File an issue](https://github.com/racklet/racklet/issues/new).
- Join our [community meetings](community/README.md).

Your feedback is always welcome!

## Maintainers

In alphabetical order:

- Dennis Marttinen, [@twelho](https://github.com/twelho)
- Jaakko Sirén, [@Jaakkonen](https://github.com/Jaakkonen)
- Lucas Käldström, [@luxas](https://github.com/luxas)
- Verneri Hirvonen, [@chiplet](https://github.com/chiplet)

## License

[Apache 2.0](LICENSE)
