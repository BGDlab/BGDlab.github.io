### Connecting to PMACS

Don't need to connect via vpn

```
$ ssh -Y [username]@scisub.pmacs.upenn.edu #SSH only. No sftp. No outbound access. Can submit or check jobs.
$ ssh -Y [username]@sciget.pmacs.upenn.edu #SSH, but has outbound access to use git, svn, wget, et cetera to download data. Can *not* submit or check jobs.
```


### Using the batch system

PMACS uses IBM’s Load Sharing Facility (LSF) to submit and monitor jobs. This is very different from SGE used on CUBIC. The biggest differences are you will use `bsub` to submit jobs and `bjobs` to monitor jobs instead of SGE’s `qsub` to submit jobs and `qstat` to monitor jobs.

The documentation for LSF can be found [here](https://www.ibm.com/support/knowledgecenter/SSWRJV_10.1.0/lsf_welcome/lsf_welcome.html).


### Submitting tickets
https://helpdesk.pmacs.upenn.edu/
