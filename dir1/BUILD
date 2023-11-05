

def create_sr(name, src):
    build_file = text_file(
        name=tag(name, "buildfile"),
        strip=True,
        content=f"""
            genrule(
                name = 'build',
                visibility=["PUBLIC"],
                binary = True,
                srcs = ['{src}'],
                out = 'bin.sh',
                cmd = "sed 's/hello/hello world/' $SRCS > $OUT",
            )
        """,
    )
    repo = build_rule(
        name=tag(name, "srcs"),
        srcs={
            "s": [src],
            "build": [build_file],
        },
        outs=["repo"],
        cmd='mkdir "$OUTS" && mv $SRCS_S "$OUTS"/ && mv "$SRCS_BUILD" "$OUTS"/BUILD',
        _subrepo=True,
    )
    subrepo(
        name='this_subrepo',
        dep=repo,
        package_root="pkg/",
    )

    pkg_name = package_name()
    return filegroup(
        name=name,
        visibility=["PUBLIC"],
        exported_deps=[f"///{pkg_name}/this_subrepo//:build"],
    )


filegroup(
    name = basename(package_name()),
    srcs = [
        create_sr(name = 'subrepo', src = './script.sh')
    ],
    visibility = ["PUBLIC"],
)
