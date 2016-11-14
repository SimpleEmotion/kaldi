cmake_minimum_required(VERSION 3.2)

include(ExternalProject)

# openfst
externalproject_add(
  openfst
  PREFIX "${CMAKE_CURRENT_SOURCE_DIR}/openfst"
  GIT_REPOSITORY "https://github.com/SimpleEmotion/openfst-binaries"
  GIT_TAG ""
  GIT_SUBMODULES ""
  CONFIGURE_COMMAND ""
  BUILD_COMMAND ""
  INSTALL_COMMAND ""
)
externalproject_get_property(openfst source_dir)
set(OPENFST_INCLUDE_DIR ${source_dir}/include CACHE STRING "OPENFST_INCLUDE_DIR" FORCE)

# Artifact output locations
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}/bin")
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}/bin")
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}/bin")

# Compile flags
set(CMAKE_CXX_FLAGS "-std=gnu++14 -march=native -O3 -DNDEBUG")

if(CYGWIN)
  set(CMAKE_CXX_FLAGS "-std=gnu++14 -march=native -O3 -Wa,-mbig-obj -DNDEBUG")
endif(CYGWIN)

# Definitions
add_definitions(-DKALDI_DOUBLEPRECISION=0)
add_definitions(-DHAVE_CLAPACK)
add_definitions(-DHAVE_OPENFST_GE_10400)

# Include headers
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/src)
include_directories(${OPENFST_INCLUDE_DIR})
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/tools/CLAPACK)

# Declare Projects
project(kaldi)

# Create Libraries
add_library(
  kaldi

  src/base/io-funcs.cc
  src/base/kaldi-error.cc
  src/base/kaldi-math.cc
  src/base/kaldi-utils.cc

  src/chain/chain-den-graph.cc
  src/chain/chain-denominator.cc
  src/chain/chain-numerator.cc
  src/chain/chain-supervision.cc
  src/chain/chain-training.cc
  src/chain/language-model.cc

  src/cudamatrix/cu-allocator.cc
  src/cudamatrix/cu-array.cc
  src/cudamatrix/cu-block-matrix.cc
  src/cudamatrix/cu-common.cc
  src/cudamatrix/cu-device.cc
  src/cudamatrix/cu-math.cc
  src/cudamatrix/cu-matrix.cc
  src/cudamatrix/cu-packed-matrix.cc
  src/cudamatrix/cu-rand.cc
  src/cudamatrix/cu-sp-matrix.cc
  src/cudamatrix/cu-sparse-matrix.cc
  src/cudamatrix/cu-tp-matrix.cc
  src/cudamatrix/cu-vector.cc

  src/decoder/decoder-wrappers.cc
  src/decoder/faster-decoder.cc
  src/decoder/lattice-faster-decoder.cc
  src/decoder/lattice-faster-online-decoder.cc
  src/decoder/lattice-simple-decoder.cc
  src/decoder/simple-decoder.cc
  src/decoder/training-graph-compiler.cc

  src/feat/feature-fbank.cc
  src/feat/feature-functions.cc
  src/feat/feature-mfcc.cc
  src/feat/feature-plp.cc
  src/feat/feature-spectrogram.cc
  src/feat/feature-window.cc
  src/feat/mel-computations.cc
  src/feat/online-feature.cc
  src/feat/pitch-functions.cc
  src/feat/resample.cc
  src/feat/signal.cc
  src/feat/sinusoid-detection.cc
  src/feat/wave-reader.cc

  src/fstext/kaldi-fst-io.cc
  src/fstext/push-special.cc

  src/gmm/am-diag-gmm.cc
  src/gmm/decodable-am-diag-gmm.cc
  src/gmm/diag-gmm-normal.cc
  src/gmm/diag-gmm.cc
  src/gmm/ebw-diag-gmm.cc
  src/gmm/full-gmm-normal.cc
  src/gmm/full-gmm.cc
  src/gmm/indirect-diff-diag-gmm.cc
  src/gmm/mle-am-diag-gmm.cc
  src/gmm/mle-diag-gmm.cc
  src/gmm/mle-full-gmm.cc
  src/gmm/model-common.cc

  src/hmm/hmm-topology.cc
  src/hmm/hmm-utils.cc
  src/hmm/posterior.cc
  src/hmm/transition-model.cc
  src/hmm/tree-accu.cc

  src/ivector/ivector-extractor.cc
  src/ivector/logistic-regression.cc
  src/ivector/plda.cc
  src/ivector/voice-activity-detection.cc

  src/kws/kws-functions.cc
  src/kws/kws-functions2.cc
  src/kws/kws-scoring.cc

  src/lat/confidence.cc
  src/lat/determinize-lattice-pruned.cc
  src/lat/kaldi-lattice.cc
  src/lat/lattice-functions.cc
  src/lat/minimize-lattice.cc
  src/lat/phone-align-lattice.cc
  src/lat/push-lattice.cc
  src/lat/sausages.cc
  src/lat/word-align-lattice-lexicon.cc
  src/lat/word-align-lattice.cc

  src/lm/arpa-file-parser.cc
  src/lm/arpa-lm-compiler.cc
  src/lm/const-arpa-lm.cc
  src/lm/kaldi-rnnlm.cc
  src/lm/mikolov-rnnlm-lib.cc

  src/matrix/compressed-matrix.cc
  src/matrix/kaldi-gpsr.cc
  src/matrix/kaldi-matrix.cc
  src/matrix/kaldi-vector.cc
  src/matrix/matrix-functions.cc
  src/matrix/optimization.cc
  src/matrix/packed-matrix.cc
  src/matrix/qr.cc
  src/matrix/sp-matrix.cc
  src/matrix/sparse-matrix.cc
  src/matrix/srfft.cc
  src/matrix/tp-matrix.cc

  src/nnet/nnet-component.cc
  src/nnet/nnet-loss.cc
  src/nnet/nnet-nnet.cc
  src/nnet/nnet-pdf-prior.cc
  src/nnet/nnet-randomizer.cc

  src/nnet2/am-nnet.cc
  src/nnet2/combine-nnet-a.cc
  src/nnet2/combine-nnet-fast.cc
  src/nnet2/combine-nnet.cc
  src/nnet2/get-feature-transform.cc
  src/nnet2/mixup-nnet.cc
  src/nnet2/nnet-component.cc
  src/nnet2/nnet-compute-discriminative.cc
  src/nnet2/nnet-compute-discriminative-parallel.cc
  src/nnet2/nnet-compute-online.cc
  src/nnet2/nnet-compute.cc
  src/nnet2/nnet-example-functions.cc
  src/nnet2/nnet-example.cc
  src/nnet2/nnet-fix.cc
  src/nnet2/nnet-functions.cc
  src/nnet2/nnet-limit-rank.cc
  src/nnet2/nnet-nnet.cc
  src/nnet2/nnet-precondition-online.cc
  src/nnet2/nnet-precondition.cc
  src/nnet2/nnet-stats.cc
  src/nnet2/nnet-update-parallel.cc
  src/nnet2/nnet-update.cc
  src/nnet2/online-nnet2-decodable.cc
  src/nnet2/rescale-nnet.cc
  src/nnet2/shrink-nnet.cc
  src/nnet2/train-nnet-ensemble.cc
  src/nnet2/train-nnet.cc
  src/nnet2/widen-nnet.cc

  src/nnet3/am-nnet-simple.cc
  src/nnet3/discriminative-supervision.cc
  src/nnet3/discriminative-training.cc
  src/nnet3/natural-gradient-online.cc
  src/nnet3/nnet-am-decodable-simple.cc
  src/nnet3/nnet-analyze.cc
  src/nnet3/nnet-chain-combine.cc
  src/nnet3/nnet-chain-diagnostics.cc
  src/nnet3/nnet-chain-example.cc
  src/nnet3/nnet-chain-training.cc
  src/nnet3/nnet-combine.cc
  src/nnet3/nnet-common.cc
  src/nnet3/nnet-compile-utils.cc
  src/nnet3/nnet-compile.cc
  src/nnet3/nnet-component-itf.cc
  src/nnet3/nnet-computation-graph.cc
  src/nnet3/nnet-computation.cc
  src/nnet3/nnet-compute.cc
  src/nnet3/nnet-descriptor.cc
  src/nnet3/nnet-diagnostics.cc
  src/nnet3/nnet-discriminative-diagnostics.cc
  src/nnet3/nnet-discriminative-example.cc
  src/nnet3/nnet-discriminative-training.cc
  src/nnet3/nnet-example-utils.cc
  src/nnet3/nnet-example.cc
  src/nnet3/nnet-general-component.cc
  src/nnet3/nnet-graph.cc
  src/nnet3/nnet-nnet.cc
  src/nnet3/nnet-optimize-utils.cc
  src/nnet3/nnet-optimize.cc
  src/nnet3/nnet-parse.cc
  src/nnet3/nnet-simple-component.cc
  src/nnet3/nnet-training.cc
  src/nnet3/nnet-utils.cc
  src/nnet3/online-nnet3-decodable-simple.cc

  #  src/online/online-audio-source.cc
  #  src/online/online-decodable.cc
  #  src/online/online-faster-decoder.cc
  #  src/online/online-feat-input.cc
  #  src/online/online-tcp-source.cc
  #  src/online/onlinebin-util.cc

  src/online2/online-endpoint.cc
  src/online2/online-feature-pipeline.cc
  src/online2/online-gmm-decodable.cc
  src/online2/online-gmm-decoding.cc
  src/online2/online-ivector-feature.cc
  src/online2/online-nnet2-decoding-threaded.cc
  src/online2/online-nnet2-decoding.cc
  src/online2/online-nnet2-feature-pipeline.cc
  src/online2/online-nnet3-decoding.cc
  src/online2/online-speex-wrapper.cc
  src/online2/online-timing.cc
  src/online2/onlinebin-util.cc

  src/sgmm/am-sgmm.cc
  src/sgmm/decodable-am-sgmm.cc
  src/sgmm/estimate-am-sgmm-ebw.cc
  src/sgmm/estimate-am-sgmm-multi.cc
  src/sgmm/estimate-am-sgmm.cc
  src/sgmm/fmllr-sgmm.cc
  src/sgmm/sgmm-clusterable.cc

  src/sgmm2/am-sgmm2-project.cc
  src/sgmm2/am-sgmm2.cc
  src/sgmm2/decodable-am-sgmm2.cc
  src/sgmm2/estimate-am-sgmm2-ebw.cc
  src/sgmm2/estimate-am-sgmm2.cc
  src/sgmm2/fmllr-sgmm2.cc

  src/thread/kaldi-barrier.cc
  src/thread/kaldi-mutex.cc
  src/thread/kaldi-semaphore.cc
  src/thread/kaldi-thread.cc

  src/transform/basis-fmllr-diag-gmm.cc
  src/transform/cmvn.cc
  src/transform/compressed-transform-stats.cc
  src/transform/decodable-am-diag-gmm-regtree.cc
  src/transform/fmllr-diag-gmm.cc
  src/transform/fmllr-raw.cc
  src/transform/fmpe.cc
  src/transform/hlda.cc
  src/transform/lda-estimate.cc
  src/transform/lvtln.cc
  src/transform/mllt.cc
  src/transform/regression-tree.cc
  src/transform/regtree-fmllr-diag-gmm.cc
  src/transform/regtree-mllr-diag-gmm.cc
  src/transform/transform-common.cc

  src/tree/build-tree-questions.cc
  src/tree/build-tree-utils.cc
  src/tree/build-tree.cc
  src/tree/cluster-utils.cc
  src/tree/clusterable-classes.cc
  src/tree/context-dep.cc
  src/tree/event-map.cc
  src/tree/tree-renderer.cc

  src/util/kaldi-holder.cc
  src/util/kaldi-io.cc
  src/util/kaldi-table.cc
  src/util/parse-options.cc
  src/util/simple-io-funcs.cc
  src/util/simple-options.cc
  src/util/text-utils.cc
)
set_target_properties(kaldi PROPERTIES PREFIX "")

# Declare Dependencies
add_dependencies(kaldi openfst)
