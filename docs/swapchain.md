# Vulkan Swapchain Quirks

### Non-Apple platforms swapchain extent/window extent mismatch
On non-apple platforms, when using high-dpi displays there's a mismath between the reported surface extent and the real window size.

In this case, be sure to query your real window's size in pixels instead of relying on `currentExtent.width` and `currentExtent.height`, clamped between `minImageExtent` and `maxImageExtent`.

Here's an example from [SDL's GPU subsystem](https://github.com/libsdl-org/SDL/blob/9da46bc37fb9920f5ee12b187f165a30339985cc/src/gpu/vulkan/SDL_gpu_vulkan.c#L9669)

### NVIDIA + Win32 invalid swapchain extent
On Win32, sometimes NVIDIA drivers will report an invalid surface with 0x0 extent.

Creating an swapchain with invalid dimensions is illegal and will crash your application!

Be sure to check `VkSurfaceCapabilitiesKHR`'s `currentExtent.width` and `currentExtent.height` for 0-values. 

Here's an example from [SDL's GPU subsystem](https://github.com/libsdl-org/SDL/blob/9da46bc37fb9920f5ee12b187f165a30339985cc/src/gpu/vulkan/SDL_gpu_vulkan.c#L4525)