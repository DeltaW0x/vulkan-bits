# Vulkan Devices Quirks

### VK_INCOMPLETE result when calling  `vkEnumeratePhysicalDevices`
Sometimes the vulkan loader will report a `VK_INCOMPLETE` code when enumerating a system's physical devices list. This is not a real error, but it's the drivers way to warn that the returned list might be smaller than the previously queried `pPhysicalDeviceCount` value due to a driver issue.

It is ok to continue sorting/selecting your devices after that, just expect a return value of vkEnumeratePhysicalDevices not being `VK_SUCCESS` without being an error

Here's an example from [SDL's GPU subsystem](https://github.com/libsdl-org/SDL/blob/9da46bc37fb9920f5ee12b187f165a30339985cc/src/gpu/vulkan/SDL_gpu_vulkan.c#L11292)