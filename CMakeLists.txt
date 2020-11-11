cmake_minimum_required(VERSION 3.0.0)
project(openh264)

option(OPENH264_USE_ASM "Build with nasm assembler" ON)

add_library(openh264_common STATIC)
target_compile_definitions(openh264_common PRIVATE _LIB=1)
target_include_directories(openh264_common
PUBLIC
    codec/api/svc
PRIVATE
    codec/common/inc
)

target_sources(openh264_common PRIVATE
    codec/common/src/common_tables.cpp
    codec/common/src/copy_mb.cpp
    codec/common/src/cpu.cpp
    codec/common/src/crt_util_safe_x.cpp
    codec/common/src/deblocking_common.cpp
    codec/common/src/expand_pic.cpp
    codec/common/src/intra_pred_common.cpp
    codec/common/src/mc.cpp
    codec/common/src/memory_align.cpp
    codec/common/src/sad_common.cpp
    codec/common/src/utils.cpp
    codec/common/src/welsCodecTrace.cpp
    codec/common/src/WelsTaskThread.cpp
    codec/common/src/WelsThread.cpp
    codec/common/src/WelsThreadLib.cpp
    codec/common/src/WelsThreadPool.cpp
)




add_library(openh264_enc STATIC)
target_compile_definitions(openh264_enc PRIVATE _LIB=1)
target_include_directories(openh264_enc
PUBLIC
    codec/api/svc
PRIVATE
    codec/common/inc
    codec/encoder/core/inc
    codec/encoder/plus/inc
    codec/processing/interface
    codec/processing/src/common
)

target_sources(openh264_enc PRIVATE
    codec/encoder/core/src/au_set.cpp
    codec/encoder/core/src/deblocking.cpp
    codec/encoder/core/src/decode_mb_aux.cpp
    codec/encoder/core/src/encode_mb_aux.cpp
    codec/encoder/core/src/encoder.cpp
    codec/encoder/core/src/encoder_data_tables.cpp
    codec/encoder/core/src/encoder_ext.cpp
    codec/encoder/core/src/get_intra_predictor.cpp
    codec/encoder/core/src/md.cpp
    codec/encoder/core/src/mv_pred.cpp
    codec/encoder/core/src/nal_encap.cpp
    codec/encoder/core/src/paraset_strategy.cpp
    codec/encoder/core/src/picture_handle.cpp
    codec/encoder/core/src/ratectl.cpp
    codec/encoder/core/src/ref_list_mgr_svc.cpp
    codec/encoder/core/src/sample.cpp
    codec/encoder/core/src/set_mb_syn_cabac.cpp
    codec/encoder/core/src/set_mb_syn_cavlc.cpp
    codec/encoder/core/src/slice_multi_threading.cpp
    codec/encoder/core/src/svc_base_layer_md.cpp
    codec/encoder/core/src/svc_enc_slice_segment.cpp
    codec/encoder/core/src/svc_encode_mb.cpp
    codec/encoder/core/src/svc_encode_slice.cpp
    codec/encoder/core/src/svc_mode_decision.cpp
    codec/encoder/core/src/svc_motion_estimate.cpp
    codec/encoder/core/src/svc_set_mb_syn_cabac.cpp
    codec/encoder/core/src/svc_set_mb_syn_cavlc.cpp
    codec/encoder/core/src/wels_preprocess.cpp
    codec/encoder/core/src/wels_task_base.cpp
    codec/encoder/core/src/wels_task_encoder.cpp
    codec/encoder/core/src/wels_task_management.cpp
    codec/encoder/plus/src/welsEncoderExt.cpp
)



add_library(openh264_dec STATIC)
target_compile_definitions(openh264_dec PRIVATE _LIB=1)
target_include_directories(openh264_dec
PUBLIC
    codec/api/svc
PRIVATE
    codec/common/inc
    codec/decoder/core/inc
    codec/decoder/plus/inc
    codec/processing/interface
    codec/processing/src/common
)

target_sources(openh264_dec PRIVATE
    codec/decoder/core/src/au_parser.cpp
    codec/decoder/core/src/bit_stream.cpp
    codec/decoder/core/src/cabac_decoder.cpp      
    codec/decoder/core/src/deblocking.cpp
    codec/decoder/core/src/decoder.cpp
    codec/decoder/core/src/decoder_core.cpp       
    codec/decoder/core/src/decoder_data_tables.cpp
    codec/decoder/core/src/decode_mb_aux.cpp      
    codec/decoder/core/src/decode_slice.cpp       
    codec/decoder/core/src/error_concealment.cpp  
    codec/decoder/core/src/fmo.cpp
    codec/decoder/core/src/get_intra_predictor.cpp
    codec/decoder/core/src/manage_dec_ref.cpp     
    codec/decoder/core/src/memmgr_nal_unit.cpp    
    codec/decoder/core/src/mv_pred.cpp
    codec/decoder/core/src/parse_mb_syn_cabac.cpp
    codec/decoder/core/src/parse_mb_syn_cavlc.cpp
    codec/decoder/core/src/pic_queue.cpp
    codec/decoder/core/src/rec_mb.cpp
    codec/decoder/plus/src/welsDecoderExt.cpp
)





add_library(openh264_proc STATIC)
target_compile_definitions(openh264_proc PRIVATE _LIB=1)
target_include_directories(openh264_proc
PUBLIC
    codec/api/svc
PRIVATE
    codec/common/inc
    codec/processing/interface
    codec/processing/src/common
)

target_sources(openh264_proc PRIVATE
    codec/processing/src/adaptivequantization/AdaptiveQuantization.cpp
    codec/processing/src/backgrounddetection/BackgroundDetection.cpp
    codec/processing/src/common/memory.cpp
    codec/processing/src/common/WelsFrameWork.cpp
    codec/processing/src/common/WelsFrameWorkEx.cpp
    codec/processing/src/complexityanalysis/ComplexityAnalysis.cpp
    codec/processing/src/denoise/denoise.cpp
    codec/processing/src/denoise/denoise_filter.cpp
    codec/processing/src/downsample/downsample.cpp
    codec/processing/src/downsample/downsamplefuncs.cpp
    codec/processing/src/imagerotate/imagerotate.cpp
    codec/processing/src/imagerotate/imagerotatefuncs.cpp
    codec/processing/src/scenechangedetection/SceneChangeDetection.cpp
    codec/processing/src/scrolldetection/ScrollDetection.cpp
    codec/processing/src/scrolldetection/ScrollDetectionFuncs.cpp
    codec/processing/src/vaacalc/vaacalcfuncs.cpp
    codec/processing/src/vaacalc/vaacalculation.cpp
)


execute_process(COMMAND ${CMAKE_COMMAND} "-E" "touch" "dummy.cpp" WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR})

add_library(openh264 SHARED
    openh264.def)
target_include_directories(openh264
PUBLIC
    codec/api/svc
)
target_link_libraries(openh264 PUBLIC openh264_common openh264_enc openh264_dec openh264_proc)


add_library(openh264_a STATIC ${CMAKE_CURRENT_BINARY_DIR}/dummy.cpp)
target_include_directories(openh264_a
PUBLIC
    codec/api/svc
)
target_link_libraries(openh264_a PUBLIC openh264_common openh264_enc openh264_dec openh264_proc)


include(CheckLanguage)
check_language(ASM_NASM)
if (NOT CMAKE_ASM_NASM_COMPILER)
    message(WARNING "No nasm found. asm disabled!")
    set(OPENH264_USE_ASM OFF)
endif ()

if (${OPENH264_USE_ASM})
    enable_language(ASM_NASM)
    
    target_sources(openh264_common PRIVATE
        codec/common/x86/cpuid.asm
        codec/common/x86/dct.asm
        codec/common/x86/deblock.asm
        codec/common/x86/expand_picture.asm
        codec/common/x86/intra_pred_com.asm
        codec/common/x86/mb_copy.asm
        codec/common/x86/mc_chroma.asm
        codec/common/x86/mc_luma.asm
        codec/common/x86/satd_sad.asm
        codec/common/x86/vaa.asm
    )
    target_sources(openh264_enc PRIVATE
        codec/encoder/core/x86/coeff.asm
        codec/encoder/core/x86/dct.asm
        codec/encoder/core/x86/intra_pred.asm
        codec/encoder/core/x86/matrix_transpose.asm
        codec/encoder/core/x86/memzero.asm
        codec/encoder/core/x86/quant.asm
        codec/encoder/core/x86/sample_sc.asm
        codec/encoder/core/x86/score.asm
    )
    target_sources(openh264_dec PRIVATE
        codec/decoder/core/x86/dct.asm
        codec/decoder/core/x86/intra_pred.asm
    )
    target_sources(openh264_proc PRIVATE
        codec/processing/src/x86/denoisefilter.asm
        codec/processing/src/x86/downsample_bilinear.asm
        codec/processing/src/x86/vaa.asm
    )
    set(CMAKE_ASM_NASM_FLAGS -I"${CMAKE_CURRENT_SOURCE_DIR}/codec/common/x86/")

    if (${CMAKE_SIZEOF_VOID_P} EQUAL 4)
        message("build on 32bit system")
        target_compile_definitions(openh264_common PRIVATE X86_ASM X86_32_ASM)
        target_compile_definitions(openh264_enc PRIVATE X86_ASM X86_32_ASM)
        target_compile_definitions(openh264_dec PRIVATE X86_ASM X86_32_ASM)
        target_compile_definitions(openh264_proc PRIVATE X86_ASM X86_32_ASM)

        set(CMAKE_ASM_NASM_FLAGS "${CMAKE_ASM_NASM_FLAGS} -DX86_32 -DPREFIX")
    elseif (${CMAKE_SIZEOF_VOID_P} EQUAL 8)
        message("build on 64bit system")
        target_compile_definitions(openh264_common PRIVATE X86_ASM)
        target_compile_definitions(openh264_enc PRIVATE X86_ASM)
        target_compile_definitions(openh264_dec PRIVATE X86_ASM)
        target_compile_definitions(openh264_proc PRIVATE X86_ASM)
        if (MICROSOFT)
            set(CMAKE_ASM_NASM_FLAGS "${CMAKE_ASM_NASM_FLAGS} -DWIN64")
        elseif (APPLE)
            set(CMAKE_ASM_NASM_FLAGS "${CMAKE_ASM_NASM_FLAGS} -DPREFIX -DUNIX64")
        endif ()
        message(WARNING "${CMAKE_ASM_NASM_FLAGS}")
    endif ()
endif ()
