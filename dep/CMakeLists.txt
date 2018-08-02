
#######################
# ReverseTale-Library

    RequireExternal(
        TARGET ReverseTale-LoginServer
        MODULE "ReverseTale/Library:master"
        INSTALL_NAME ReverseTale-Library
    )

#######################
# ReverseTale-Common-Server

    RequireExternal(
        TARGET ReverseTale-LoginServer
        MODULE "ReverseTale/Common-Server:master"
        INSTALL_NAME ReverseTale-CommonServer
        ENSURE_ORDER
    )
