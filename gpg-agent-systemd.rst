Many of my environmental problems stem from attempting to detect
whether the gpg-agent is running from within my ~/.bashrc, taking
different actions based on the results of the detection.  My problems
are the result of incorrectly interpreting results of the detection
and setting improper environment variable values.  As of Debian
Stretch, the gnupg-agent package ships with configuration for
user-mode systemd integration.  To enable user-mode systemd, the
libpam-systemd package must be installed and the user must create a
new session.  This can be accomplished by stopping gdm3 and
re-starting it, or if the user is only logged in via a vty, logging in
on another vty should be sufficient.

  $ systemctl --user status gpg-agent-ssh.socket
  ● gpg-agent-ssh.socket - GnuPG cryptographic agent (ssh-agent emulation)
     Loaded: loaded (/usr/lib/systemd/user/gpg-agent-ssh.socket; disabled; vendor preset: enabled)
     Active: active (running) since Mon 2017-04-24 22:23:13 PDT; 6 days ago
       Docs: man:gpg-agent(1)
             man:ssh-add(1)
             man:ssh-agent(1)
             man:ssh(1)
     Listen: /run/user/1000/gnupg/S.gpg-agent.ssh (Stream)

  Apr 24 22:23:13 probook0 systemd[2969]: Listening on GnuPG cryptographic agent (ssh-agent emulation).

Once the service is active, the SSH_AUTH_SOCK,
/run/user/1000/gnupg/S.gpg-agent.ssh will be available for use.  Since
this path does not change depending on environment, once can simply
add the following to the end of their ~/.bashrc file:

  export SSH_AUTH_SOCK=/run/user/1000/gnupg/S.gpg-agent.ssh
