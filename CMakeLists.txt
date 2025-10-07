cmake_minimum_required(VERSION 3.16)

# UTF-8 source
add_compile_options("$<$<AND:$<C_COMPILER_ID:MSVC>,$<COMPILE_LANGUAGE:C>>:/utf-8>")
add_compile_options("$<$<AND:$<CXX_COMPILER_ID:MSVC>,$<COMPILE_LANGUAGE:CXX>>:/utf-8>")
add_compile_options("$<$<AND:$<CXX_COMPILER_ID:MSVC>,$<COMPILE_LANGUAGE:CXX>>:/Zc:__cplusplus>")

set(PROJECT_NAME krglhwebp)
set(PROJECT_VERSION 1.00)

project(${PROJECT_NAME} VERSION ${PROJECT_VERSION} LANGUAGES C CXX)

set(WEBP_SOURCES
    libwebp/sharpyuv/sharpyuv.c
    libwebp/sharpyuv/sharpyuv_csp.c
    libwebp/sharpyuv/sharpyuv_dsp.c
    libwebp/sharpyuv/sharpyuv_gamma.c
    libwebp/src/dec/alpha_dec.c
    libwebp/src/dec/buffer_dec.c
    libwebp/src/dec/frame_dec.c
    libwebp/src/dec/idec_dec.c
    libwebp/src/dec/io_dec.c
    libwebp/src/dec/quant_dec.c
    libwebp/src/dec/tree_dec.c
    libwebp/src/dec/vp8_dec.c
    libwebp/src/dec/vp8l_dec.c
    libwebp/src/dec/webp_dec.c
    libwebp/src/dsp/alpha_processing.c
    libwebp/src/dsp/cpu.c
    libwebp/src/dsp/dec.c
    libwebp/src/dsp/dec_clip_tables.c
    libwebp/src/dsp/filters.c
    libwebp/src/dsp/lossless.c
    libwebp/src/dsp/rescaler.c
    libwebp/src/dsp/upsampling.c
    libwebp/src/dsp/yuv.c
    libwebp/src/dsp/cost.c
    libwebp/src/dsp/enc.c
    libwebp/src/dsp/lossless_enc.c
    libwebp/src/dsp/ssim.c
    libwebp/src/enc/alpha_enc.c
    libwebp/src/enc/analysis_enc.c
    libwebp/src/enc/backward_references_cost_enc.c
    libwebp/src/enc/backward_references_enc.c
    libwebp/src/enc/config_enc.c
    libwebp/src/enc/cost_enc.c
    libwebp/src/enc/filter_enc.c
    libwebp/src/enc/frame_enc.c
    libwebp/src/enc/histogram_enc.c
    libwebp/src/enc/iterator_enc.c
    libwebp/src/enc/near_lossless_enc.c
    libwebp/src/enc/picture_enc.c
    libwebp/src/enc/picture_csp_enc.c
    libwebp/src/enc/picture_psnr_enc.c
    libwebp/src/enc/picture_rescale_enc.c
    libwebp/src/enc/picture_tools_enc.c
    libwebp/src/enc/predictor_enc.c
    libwebp/src/enc/quant_enc.c
    libwebp/src/enc/syntax_enc.c
    libwebp/src/enc/token_enc.c
    libwebp/src/enc/tree_enc.c
    libwebp/src/enc/vp8l_enc.c
    libwebp/src/enc/webp_enc.c
    libwebp/src/demux/anim_decode.c
    libwebp/src/demux/demux.c
    libwebp/src/mux/anim_encode.c
    libwebp/src/mux/muxedit.c
    libwebp/src/mux/muxinternal.c
    libwebp/src/mux/muxread.c
    libwebp/src/utils/bit_reader_utils.c
    libwebp/src/utils/color_cache_utils.c
    libwebp/src/utils/filters_utils.c
    libwebp/src/utils/huffman_utils.c
    libwebp/src/utils/quant_levels_dec_utils.c
    libwebp/src/utils/rescaler_utils.c
    libwebp/src/utils/random_utils.c
    libwebp/src/utils/thread_utils.c
    libwebp/src/utils/utils.c
    libwebp/src/utils/bit_writer_utils.c
    libwebp/src/utils/huffman_encode_utils.c
    libwebp/src/utils/quant_levels_utils.c
)

set(WEBP_SSE2_SOURCES
    libwebp/sharpyuv/sharpyuv_sse2.c
    libwebp/src/dsp/alpha_processing_sse2.c
    libwebp/src/dsp/dec_sse2.c
    libwebp/src/dsp/filters_sse2.c
    libwebp/src/dsp/lossless_sse2.c
    libwebp/src/dsp/rescaler_sse2.c
    libwebp/src/dsp/upsampling_sse2.c
    libwebp/src/dsp/yuv_sse2.c
    libwebp/src/dsp/cost_sse2.c
    libwebp/src/dsp/enc_sse2.c
    libwebp/src/dsp/lossless_enc_sse2.c
    libwebp/src/dsp/ssim_sse2.c
)

set(WEBP_SSE41_SOURCES
    libwebp/src/dsp/alpha_processing_sse41.c
    libwebp/src/dsp/dec_sse41.c
    libwebp/src/dsp/lossless_sse41.c
    libwebp/src/dsp/upsampling_sse41.c
    libwebp/src/dsp/yuv_sse41.c
    libwebp/src/dsp/enc_sse41.c
    libwebp/src/dsp/lossless_enc_sse41.c
)

set(WEBP_NEON_SOURCES
    libwebp/sharpyuv/sharpyuv_neon.c
    libwebp/src/dsp/alpha_processing_neon.c
    libwebp/src/dsp/cost_neon.c
    libwebp/src/dsp/dec_neon.c
    libwebp/src/dsp/enc_neon.c
    libwebp/src/dsp/filters_neon.c
    libwebp/src/dsp/lossless_enc_neon.c
    libwebp/src/dsp/lossless_neon.c
    libwebp/src/dsp/rescaler_neon.c
    libwebp/src/dsp/upsampling_neon.c
    libwebp/src/dsp/yuv_neon.c
)

option(KRGLH_WEBP_ENABLE_INTEL "Enable Intel-specific optimizations (SSE2, SSE4.1)" ON)
option(KRGLH_WEBP_ENABLE_ARM "Enable ARM-specific optimizations (NEON)" OFF)

set(ARCH_SPECIFIC_SOURCES "")
set(ARCH_SPECIFIC_DEFINITIONS "")

if(KRGLH_WEBP_ENABLE_INTEL)
    list(APPEND ARCH_SPECIFIC_SOURCES ${WEBP_SSE2_SOURCES} ${WEBP_SSE41_SOURCES})
    list(APPEND ARCH_SPECIFIC_DEFINITIONS WEBP_HAVE_SSE2 WEBP_HAVE_SSE41)
    set_source_files_properties(${WEBP_SSE2_SOURCES} PROPERTIES COMPILE_FLAGS "-msse2")
    set_source_files_properties(${WEBP_SSE41_SOURCES} PROPERTIES COMPILE_FLAGS "-msse4.1")
elseif(KRGLH_WEBP_ENABLE_ARM)
    list(APPEND ARCH_SPECIFIC_SOURCES ${WEBP_NEON_SOURCES})
    # 可能需要为 NEON 添加特定的编译选项，如 -mfpu=neon，具体取决于工具链
endif()

add_library(${PROJECT_NAME} SHARED
    dllmain.cpp
    ${WEBP_SOURCES}
    ${ARCH_SPECIFIC_SOURCES}
)

target_include_directories(${PROJECT_NAME} PRIVATE
    libwebp
    libwebp/src
)

target_compile_definitions(${PROJECT_NAME} PRIVATE
    ${ARCH_SPECIFIC_DEFINITIONS}
)

set_target_properties(${PROJECT_NAME} PROPERTIES
    CXX_STANDARD 11
    CXX_STANDARD_REQUIRED ON
    C_STANDARD 99
    C_STANDARD_REQUIRED ON
)

if(NOT TARGET tp_stub)
    add_subdirectory(../tp_stub ${CMAKE_CURRENT_BINARY_DIR}/tp_stub)
endif()

target_link_libraries(${PROJECT_NAME} PUBLIC
    tp_stub
)
