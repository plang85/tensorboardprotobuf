include(FindProtobuf)
find_package(Protobuf REQUIRED)

file(GLOB ProtoFiles "${CMAKE_CURRENT_SOURCE_DIR}/tensorflow/*.proto")
protobuf_generate_cpp(PROTO_SRC PROTO_HEADER ${ProtoFiles})
set(SOURCES record.cpp record.h run.cpp run.h utils.cpp utils.h  tensorflow/crc32c.h tensorflow/crc32c.cpp)
set(INCLUDE_DIRS ${CMAKE_CURRENT_SOURCE_DIR} ${PROTOBUF_INCLUDE_DIR} ${CMAKE_CURRENT_BINARY_DIR})
add_library(tbproto SHARED ${PROTO_HEADER} ${PROTO_SRC} ${SOURCES})
target_include_directories(tbproto PRIVATE ${INCLUDE_DIRS})
target_link_libraries(tbproto ${Protobuf_LIBRARIES})


