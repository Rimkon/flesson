
cmake_minimum_required (VERSION 3.2)                                            #

               project (my_tutor_first VERSION 0.0.1$ENV{TRAVIS_BULD_NUMBER})   #

if (NOT TARGET spdlog)                                                   
          find_package (spdlog CONFIG REQUIRED )                                  
endif ()                                                                 

if (spdlog_FOUND)
               message (STATUS "   статус spdlog: ${spdlog_FOUND}")
               message (STATUS "   путь к spdlog: ${spdlog_DIR}")
               message (STATUS " congig к spdlog: ${spdlog_CONFIG}")
               message (STATUS "congigs к spdlog: ${spdlog_CONSIDERED_CONFIGS}")
               message (STATUS "congigs к spdlog: ${spdlog_CONSIDERED_VERSIONS}")
endif()
if (NOT TARGET spdlog_FOUND)                                                   
          message (STATUS "   статус spdlog: ${spdlog_FOUND}")                                  
endif ()   

          find_package (Threads REQUIRED)                                       # 
          find_package (Boost   COMPONENTS unit_test_framework REQUIRED)        # 

link_directories(${LINK_DIRECTORIES} ${FUSE_LIBRARY_DIR})

        configure_file (version.h.in ${PROJECT_SOURCE_DIR}/version.h)           # 

           add_library (helloworld_lib            lib.cpp)                      # 
        add_executable (helloworld_for_client    main.cpp)                      # 
        add_executable (helloworld_test  test_version.cpp)                      # 

 set_target_properties (helloworld_for_client helloworld_lib                    # 
                       PROPERTIES                                               # 
                       CXX_STANDARD 17                                          # 
                       CXX_STANDARD_REQUIRED ON                                 # 
                       COMPILE_OPTIONS "-Wpedantic;-Wall;-Wextra"               # 
                       )                                                        # 

 set_target_properties (helloworld_test PROPERTIES                              # 
                       COMPILE_DEFINITIONS BOOST_TEST_DYN_LINK                  # 
                       INCLUDE_DIRECTORIES ${Boost_INCLUDE_DIR}                 # 
                       )                                                        # 
                                            
                                             
 target_link_libraries (helloworld_for_client                                   # 
                       helloworld_lib                                           # 
                       Threads::Threads                                         # 
                       )                                                        # 
 target_link_libraries (helloworld_test                                         # 
                       ${Boost_LIBRARIES}                                       # 
                       helloworld_lib                                           # 
                       )                                                        # 
              install (TARGETS  helloworld_for_client                           # 
                       RUNTIME DESTINATION bin                                  # 
                       )                                                        #  

set ( CPACK_GENERATOR  DEB )                                                    # 
                                                                   
set ( CPACK_PACKAGE_VERSION_MAJOR     "${PROJECT_VERSION_MAJOR}" )              #  
set ( CPACK_PACKAGE_VERSION_MINOR     "${PROJECT_VERSION_MINOR}" )              # 
set ( CPACK_PACKAGE_VERSION_PATCH     "${PROJECT_VERSION_PATCH}" )              # 

set ( CPACK_PACKAGE_CONTACT           rassant@mail.ru )                         # 
set ( CPACK_DEBIAN_PACKAGE_SHLIBDEPS  ON )                                      # 

       include ( CPack )                                                        # 
enable_testing ( )                                                              #  
      add_test ( helloworld_test test_version )                                 # 

