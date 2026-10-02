# PCIe Bandwidth and Compute Unlock

Orchestrates PCIe Gen2 link negotiation, eFuse hardware protection, and Tensor Core compute enablement across NVIDIA CMP mining cards.

## Language

**LinkNegotiator**:
The deep module orchestrating PCIe link retrain, profile eFuse enforcement, MMIO shadow registers, and hardware recovery across GPU endpoints.
_Avoid_: LinkManager, PcieHandler, SpeedSetter

**ComputeInspector**:
The deep module validating BAR0 BOOT_0 architecture identity and decoding Tensor Core hardware unlock state (SS0/SS1).
_Avoid_: TensorChecker, ComputeStatus, GpuAuditor

**HardwareBus**:
The hardware abstraction seam isolating physical PCI configuration and MMIO access from negotiation logic.
_Avoid_: DriverBus, IoBridge, HardwareService

**StatusContract**:
The structured typed communication protocol exchanging execution verdicts between the Go engine and consuming shell scripts.
_Avoid_: StatusLog, ResultFile, Gen2Output

**GPU Profile**:
The configuration entity defining silicon architecture boundaries, maximum allowable PCIe generations, and firmware unlock requirements for a device ID.
_Avoid_: CardConfig, DeviceInfo, ModelSpec

**eFuse Lock**:
Physical hardware silicon bit cut restricting the maximum achievable link speed on specific dies (e.g. TU116 bit 3 limited to 5.0 GT/s).
_Avoid_: HardCap, SiliconFuse, HardwareBlock

**Shadow Sequence**:
The ordered table of private MMIO register writes applied to GPU BAR0 to unlock PCIe link capabilities before retrain.
_Avoid_: RegPatch, MagicSequence, MMIOTable

**Stage 2 Recovery**:
The fallback sequence combining Root Port Link Disable or PnP device reset with register re-injection when standard link retrain fails to achieve target speed.
_Avoid_: HardReset, ForceRecover, FallbackHack

**MRRS 512B**:
Maximum Read Request Size optimization configured in PCI Device Control to prevent DMA memory fragmentation and maximize bus throughput.
_Avoid_: BufferTweak, DevctlHack, DmaBoost
