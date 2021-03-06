# Copyright Amazon.com, Inc. or its affiliates. All Rights Reserved.
#
# Licensed under the Apache License, Version 2.0 (the "License"). You may not use
# this file except in compliance with the License. A copy of the License is
# located at
#
#     http://aws.amazon.com/apache2.0/
#
# or in the "license" file accompanying this file. This file is distributed on an
# "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or
# implied. See the License for the specific language governing permissions and
# limitations under the License.

###########
# if Makefile.local exists, use it. This provides a way to override the defaults 
sinclude ../Makefile.local
#otherwise, use the default values
include ../Makefile.local_default
include ../Makefile.aws_byte_buf
#########
# Commitment values are 32 bytes long.
COMMITMENT_BUF_SIZE = 32
# In aws_cryptosdk_private_commitment_eq, two commitment values are
# compared in a loop with (32 / sizeof(uintptr_t)) iterations.
# (32 / sizeof(uintptr_t)) equals 4 since sizeof(uintptr_t) is 8UL
COMMITMENT_EQ_ITERATIONS = 4
#########
PROOF_UID = aws_cryptosdk_private_commitment_eq
HARNESS_ENTRY = $(PROOF_UID)_harness
HARNESS_FILE = $(HARNESS_ENTRY).c

CBMCFLAGS += 

DEFINES += -DCOMMITMENT_BUF_SIZE=$(COMMITMENT_BUF_SIZE)

PROJECT_SOURCES += $(CBMC_ROOT)/aws-c-common/source/byte_buf.c
PROJECT_SOURCES += $(SRCDIR)/source/cipher.c

PROOF_SOURCES += $(COMMON_PROOF_SOURCE)/make_common_data_structures.c
PROOF_SOURCES += $(COMMON_PROOF_SOURCE)/proof_allocators.c
PROOF_SOURCES += $(COMMON_PROOF_SOURCE)/utils.c
PROOF_SOURCES += $(PROOFDIR)/$(HARNESS_FILE)
PROOF_SOURCES += $(PROOF_SOURCE)/utils.c

UNWINDSET += aws_cryptosdk_private_commitment_eq.0:$(call addone, $(COMMITMENT_EQ_ITERATIONS))
UNWINDSET += aws_byte_buf_contents_match.0:$(call addone, $(COMMITMENT_BUF_SIZE))
###########

include ../Makefile.common
