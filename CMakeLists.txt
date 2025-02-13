
cmake_minimum_required(VERSION 3.4.1)

set(CMAKE_CXX_STANDARD 14)
set(CMAKE_CXX_STANDARD_REQUIRED YES)

project(TEST)
enable_testing()

#Find openSSL
find_package(OpenSSL REQUIRED)

#Prep ourselves for compiling boost
find_package(Boost COMPONENTS 
    unit_test_framework REQUIRED
    atomic REQUIRED
    chrono REQUIRED
    context REQUIRED
    date_time REQUIRED
    exception REQUIRED
    filesystem REQUIRED
    graph REQUIRED
    iostreams REQUIRED
    math_c99 REQUIRED
    math_c99f REQUIRED
    math_c99l REQUIRED
    math_tr1 REQUIRED
    math_tr1f REQUIRED
    math_tr1l REQUIRED
    prg_exec_monitor REQUIRED
    program_options REQUIRED
    random REQUIRED
    regex REQUIRED
    serialization REQUIRED
    system REQUIRED
    test_exec_monitor REQUIRED
    thread REQUIRED
    timer REQUIRED
    wave REQUIRED
    wserialization REQUIRED
)

include_directories(${Boost_INCLUDE_DIRS})
include_directories("src")
include_directories("test")
include_directories("src/contrib")

set(BELDEX_CORE_CPP "src/beldex-core-cpp")
set(BELDEX_CORE_CPP_SRC "${BELDEX_CORE_CPP}/src")
include_directories("${BELDEX_CORE_CPP}/src")
#
set(BELDEX_SRC "${BELDEX_CORE_CPP}/contrib/beldex-core-custom")
include_directories(${BELDEX_SRC})
include_directories("${BELDEX_SRC}/epee/include")
include_directories("${BELDEX_SRC}/common")
include_directories("${BELDEX_SRC}/vtlogger")
include_directories("${BELDEX_SRC}/crypto")
include_directories("${BELDEX_SRC}/cryptonote_basic")
include_directories("${BELDEX_SRC}/multisig")
include_directories("${BELDEX_SRC}/cryptonote_core")
include_directories("${BELDEX_SRC}/cryptonote_protocol")
include_directories("${BELDEX_SRC}/wallet")
include_directories("${BELDEX_SRC}/rpc")
include_directories("${BELDEX_SRC}/mnemonics")
include_directories("${BELDEX_SRC}/contrib/libsodium/include") # support sodium/… paths
include_directories("${BELDEX_SRC}/contrib/libsodium/include/sodium")


# keeping test files in a separate source directory 
file(GLOB TEST_SRCS RELATIVE ${CMAKE_CURRENT_SOURCE_DIR} test/test_*.cpp)
set(
    SRC_FILES
    #
    test/tests_common.hpp
    #
	src/Dispatch/Dispatch_Interface.hpp
	src/Dispatch/Dispatch.asio.hpp
	src/UserIdle/UserIdle.hpp
	src/UserIdle/UserIdle.cpp
    src/Passwords/PasswordController.hpp
    src/Passwords/PasswordController.cpp
	src/Settings/SettingsProviders.hpp
	src/Settings/SettingsProviders.cpp
    src/Settings/SettingsController.hpp
    src/Settings/SettingsController.cpp
	src/Lists/PersistedObjectListController.hpp
	src/Lists/PersistedObjectListController.cpp
	src/Wallets/Wallet.hpp
	src/Wallets/Wallet.cpp
	src/Wallets/WalletsListController_Base.hpp
	src/Wallets/WalletsListController_Base.cpp
	src/Wallets/WalletsListController.Full.hpp
	src/Wallets/WalletsListController.Full.cpp
	src/Wallets/WalletsListController.Lite.hpp
	src/Wallets/WalletsListController.Lite.cpp
	src/Wallets/Wallet_HostPollingController.hpp
	src/Wallets/Wallet_HostPollingController.cpp
	src/Wallets/Wallet_TxCleanupController.hpp
	src/Wallets/Wallet_TxCleanupController.cpp
	src/Wallets/Wallet_KeyImageCache.hpp
	src/Wallets/Wallet_KeyImageCache.cpp
    src/APIClient/root_certificates.hpp
    src/Currencies/Currencies.hpp
    src/Currencies/Currencies.cpp
    src/Persistence/document_persister.hpp
    src/Persistence/document_persister.cpp
    src/Persistence/PersistableObject.hpp
    src/Persistence/PersistableObject.cpp
    src/AppBridge/AppBridge.hpp
    src/AppBridge/AppBridge.cpp
    src/AppBridge/HTTPRequests_Handle_bridge.hpp
    src/App/AppServiceLocator.hpp
    src/App/AppServiceLocator.asio.cpp
    src/SendFunds/Controllers/SendFundsFormSubmissionController.hpp
    src/SendFunds/Controllers/SendFundsFormSubmissionController.cpp
    src/APIClient/HTTPRequests_Interface.hpp
    src/APIClient/HTTPRequests.beast.hpp
    src/APIClient/HostedBeldex.cpp
    src/APIClient/HostedBeldex.hpp
	src/APIClient/parsing.hpp
	src/OpenAlias/OpenAlias.hpp
	src/OpenAlias/OpenAlias.cpp
    src/RNCryptor-C/rncryptor_c.h
    src/RNCryptor-C/rncryptor_c.c
    src/RNCryptor-C/mutils.h
    src/RNCryptor-C/mutils.c
    src/base64/base64.hpp
    #
    src/contrib/rapidjson_defines.hpp
	#
	${BELDEX_CORE_CPP_SRC}/beldex_address_utils.hpp
	${BELDEX_CORE_CPP_SRC}/beldex_address_utils.cpp
	${BELDEX_CORE_CPP_SRC}/beldex_paymentID_utils.hpp
	${BELDEX_CORE_CPP_SRC}/beldex_paymentID_utils.cpp
	${BELDEX_CORE_CPP_SRC}/beldex_key_image_utils.hpp
	${BELDEX_CORE_CPP_SRC}/beldex_key_image_utils.cpp
	${BELDEX_CORE_CPP_SRC}/beldex_fee_utils.hpp
	${BELDEX_CORE_CPP_SRC}/beldex_fee_utils.cpp
	${BELDEX_CORE_CPP_SRC}/beldex_transfer_utils.hpp
	${BELDEX_CORE_CPP_SRC}/beldex_transfer_utils.cpp
	${BELDEX_CORE_CPP_SRC}/beldex_fork_rules.hpp
	${BELDEX_CORE_CPP_SRC}/beldex_fork_rules.cpp
	${BELDEX_CORE_CPP_SRC}/beldex_wallet_utils.hpp
	${BELDEX_CORE_CPP_SRC}/beldex_wallet_utils.cpp
	${BELDEX_CORE_CPP_SRC}/serial_bridge_index.hpp
	${BELDEX_CORE_CPP_SRC}/serial_bridge_index.cpp
	${BELDEX_CORE_CPP_SRC}/beldex_send_routine.hpp
	${BELDEX_CORE_CPP_SRC}/beldex_send_routine.cpp
	${BELDEX_CORE_CPP_SRC}/serial_bridge_utils.hpp
	${BELDEX_CORE_CPP_SRC}/serial_bridge_utils.cpp
	${BELDEX_CORE_CPP_SRC}/tools__ret_vals.hpp
	${BELDEX_CORE_CPP_SRC}/tools__ret_vals.cpp
	#
	${BELDEX_SRC}/cryptonote_basic/cryptonote_basic_impl.cpp
	${BELDEX_SRC}/cryptonote_basic/account.cpp
	${BELDEX_SRC}/cryptonote_basic/cryptonote_format_utils.cpp
    ${BELDEX_SRC}/cryptonote_basic/cryptonote_format_utils_basic.cpp
	${BELDEX_SRC}/crypto/crypto.cpp
	${BELDEX_SRC}/crypto/hash.c
	${BELDEX_SRC}/crypto/slow-hash-dummied.cpp
	${BELDEX_SRC}/crypto/oaes_lib.c
	${BELDEX_SRC}/crypto/crypto-ops.c
	${BELDEX_SRC}/crypto/crypto-ops-data.c
	${BELDEX_SRC}/crypto/keccak.c
	${BELDEX_SRC}/crypto/chacha.c
	${BELDEX_SRC}/crypto/random.c
	${BELDEX_SRC}/crypto/aesb.c
	${BELDEX_SRC}/crypto/tree-hash.c
	${BELDEX_SRC}/crypto/hash-extra-blake.c
	${BELDEX_SRC}/crypto/blake256.c
	${BELDEX_SRC}/crypto/hash-extra-groestl.c
	${BELDEX_SRC}/crypto/hash-extra-jh.c
	${BELDEX_SRC}/crypto/hash-extra-skein.c
	${BELDEX_SRC}/crypto/groestl.c
	${BELDEX_SRC}/crypto/jh.c
	${BELDEX_SRC}/crypto/skein.c
	${BELDEX_SRC}/cryptonote_core/cryptonote_tx_utils.cpp
	${BELDEX_SRC}/common/base58.cpp
	${BELDEX_SRC}/common/threadpool.cpp
	${BELDEX_SRC}/common/aligned.c
	${BELDEX_SRC}/common/util.cpp
	${BELDEX_SRC}/epee/src/hex.cpp
	${BELDEX_SRC}/epee/src/string_tools.cpp
	${BELDEX_SRC}/epee/src/memwipe.c
	${BELDEX_SRC}/epee/src/mlocker.cpp
	${BELDEX_SRC}/epee/src/wipeable_string.cpp
	${BELDEX_SRC}/device/device.cpp
	${BELDEX_SRC}/device/device_default.cpp
	${BELDEX_SRC}/ringct/rctOps.cpp
	${BELDEX_SRC}/ringct/rctTypes.cpp
	${BELDEX_SRC}/ringct/rctCryptoOps.c
	${BELDEX_SRC}/ringct/rctSigs.cpp
	${BELDEX_SRC}/ringct/bulletproofs.cc
    ${BELDEX_SRC}/ringct/bulletproofs_plus.cc
	${BELDEX_SRC}/ringct/multiexp.cc
	${BELDEX_SRC}/mnemonics/electrum-words.cpp
	${BELDEX_SRC}/vtlogger/logger.cpp
	${BELDEX_SRC}/contrib/libsodium/src/crypto_verify/verify.c
)
# needed for slow-hash
add_compile_options(-maes)
add_compile_definitions(BELDEX_CORE_CUSTOM)

#Run through each source
foreach(testSrc ${TEST_SRCS})
    # extract the filename without an extension (NAME_WE)
    get_filename_component(testName ${testSrc} NAME_WE)
    add_executable(
        ${testName}
        ${testSrc}
        ${SRC_FILES}
    )
    target_link_libraries(
        ${testName}
        #
        OpenSSL::SSL
        ${Boost_LIBRARIES} 
    )
    set_target_properties(
        ${testName} PROPERTIES 
        RUNTIME_OUTPUT_DIRECTORY
        ${CMAKE_CURRENT_SOURCE_DIR}/build/products
    )
    add_test(
        NAME ${testName} 
        WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/build/products 
        COMMAND ${CMAKE_CURRENT_SOURCE_DIR}/build/products/${testName} --catch_system_error=yes
    )
endforeach(testSrc)
