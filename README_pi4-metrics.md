# How to add CPU temperature metrics
Add the following lines to `./telegraf/telegraf.conf` at the end of the INPUTS sections:
```
[[inputs.file]] 
  files = ["/sys/class/thermal/thermal_zone0/temp"]
  name_override = "cpu_temperature"
  data_format = "value"
  data_type = "integer"
  
[[inputs.exec]]
  commands = [ "/opt/vc/bin/vcgencmd measure_temp" ]
  name_override = "gpu_temperature"
  data_format = "grok"
  grok_patterns = ["%{NUMBER:value:float}"]
```
Restart telegraf container (move to root folder of repository, i.e. location of `docker-compose`):
>`$ docker-compose restart telegraf`

Run test on telegraf container to check the addition:
```docker
docker exec -it telegraf \
                telegraf -config /etc/telegraf/telegraf.conf -test
```

## Appendix: vcgencmd commands for Raspberry Pi
```bash
$ sudo vcgencmd commands
commands="vcos, ap_output_control, ap_output_post_processing, vchi_test_init, vchi_test_exit, pm_set_policy, pm_get_status, pm_show_stats, pm_start_logging, pm_stop_logging, version, commands, set_vll_dir, set_backlight, set_logging, get_lcd_info, arbiter, cache_flush, otp_dump, test_result, codec_enabled, get_camera, get_mem, measure_clock, measure_volts, enable_clock, scaling_kernel, scaling_sharpness, get_hvs_asserts, get_throttled, measure_temp, get_config, hdmi_ntsc_freqs, hdmi_adjust_clock, hdmi_status_show, hvs_update_fields, pwm_speedup, force_audio, hdmi_stream_channels, hdmi_channel_map, display_power, read_ring_osc, memtest, dispmanx_list, get_rsts, schmoo, render_bar, disk_notify, inuse_notify, sus_suspend, sus_status, sus_is_enabled, sus_stop_test_thread, egl_platform_switch, mem_validate, mem_oom, mem_reloc_stats, hdmi_cvt, hdmi_timings, readmr, pmicrd, pmicwr, bootloader_version, bootloader_config, file, vctest_memmap, vctest_start, vctest_stop, vctest_set, vctest_get"
```
- vcos
- get_camera
- get_mem
- measure_clock
- measure_volts
- pwm_speedup
- display_power
- read_ring_osc
- memtest
