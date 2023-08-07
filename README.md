# my lf config

This lf is set to imitate my previous ranger settings.

## features

1. cd on quit like ranger-quit_cd_wd

If set according to lf wiki, lfcd will implement cd on quit very well. However, there are times when we don't know whether we'd like to change to a subdirectory at the time of invoking "lf", so I always have a mental overhead of deciding whether I should type "lf" or "lfcd" at the console. The following is the code to implement cd on quit like ranger-quit_cd_wd. In other words, after executing lf, decide whether to cd on quit. This is from [https://github.com/gokcehan/lf/issues/785#issuecomment-1133989624](https://github.com/gokcehan/lf/issues/785#issuecomment-1133989624). Thanks @Barbaross93

In order to realize the function, you need to write the following content in .bashrc or .bash_alias

```bash
lfcd() {
	local tempfile="$(mktemp)"
	command lf -command "map q \$echo \$PWD >$tempfile; lf -remote \"send \$id quit\"" "$@"

	if [[ -f "$tempfile" ]] && [[ "$(cat -- "$tempfile")" != "$(echo -n $(pwd))" ]]; then
		cd -- "$(cat "$tempfile")" || return
	fi
	command rm -f -- "$tempfile" 2>/dev/null
}
alias lf=lfcd
```

2. dragon (Based on dragon, add the ability to drag and drop files for lf)

This feature depends on [dragon](https://github.com/mwh/dragon), please install it.

3. Open the file according to mime-type, or open it with $EDITOR

From [https://github.com/nigo81/Lf-config](https://github.com/nigo81/Lf-config)

4. ranger like features and mappings

* Create symbolic link(fullpath link or relative to the current directory link)

* Bulk rename

* Extract or create archive

* Executing shell command in lf

* ...
