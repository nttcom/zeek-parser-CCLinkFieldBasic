spicy_add_analyzer(
    NAME cc_link_basic
    PACKAGE_NAME spicy_cc_link_basic
    SOURCES cc_link_basic.spicy cc_link_basic.evt zeek_cc_link_basic.spicy
    SCRIPTS __load__.zeek main.zeek dpd.sig
)
