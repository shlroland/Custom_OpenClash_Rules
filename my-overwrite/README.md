# My OpenClash Remote Overwrite

Chinese version: [README.zh-CN.md](README.zh-CN.md)

## Current Usage

Use this flow for the current iStoreOS setup:

```text
upstream subscriptions
  -> Sub-Store collection URL
  -> local subconverter URL using my-cfg/Custom_Clash.ini
  -> OpenClash remote overwrite EN_KEY1
```

The OpenClash remote overwrite module should be:

```text
https://raw.githubusercontent.com/shlroland/Custom_OpenClash_Rules/refs/heads/main/my-overwrite/IStoreOS_Current_ConfigURL.conf
```

In OpenClash:

1. Open remote overwrite modules.
2. Add a new module:
   - Type: `http`
   - URL: use the `IStoreOS_Current_ConfigURL.conf` URL above.
3. Add environment variable:
   - `EN_KEY1`: final Clash/Mihomo YAML URL returned by your local converter.
4. Save and apply.

`EN_KEY1` should be the final URL returned by your local converter, not the raw Sub-Store collection URL. It should already include:

- the Sub-Store aggregate subscription as the converter `url` input
- this repo's conversion template as the converter `config` input:

```text
https://raw.githubusercontent.com/shlroland/Custom_OpenClash_Rules/refs/heads/main/my-cfg/Custom_Clash.ini
```

Shape of the local converter URL:

```text
http://<local-converter-host>:<port>/sub?target=clash&url=<urlencoded-sub-store-collection-url>&config=<urlencoded-template-url>
```

This keeps OpenClash responsible only for plugin-side settings and downloading the final YAML. Sub-Store and the local converter own subscription aggregation and template conversion.

## Overview

This folder contains OpenClash remote overwrite modules and a final Clash/Mihomo YAML config.

## Files

- `Custom_Overwrite_Multi.conf`: OpenClash remote overwrite module.
- `Custom_Overwrite_ConfigURL.conf`: OpenClash remote overwrite module that downloads a fully converted config URL from `EN_KEY1`.
- `IStoreOS_Current_ConfigURL.conf`: Config-URL overwrite module matching the inspected iStoreOS OpenClash settings at `192.168.20.10`.
- `Custom_Clash.yaml`: final Clash/Mihomo config downloaded by the overwrite module.

## Remote URLs

Recommended overwrite module when `EN_KEY1` is already a converted Clash/Mihomo config URL:

```text
https://raw.githubusercontent.com/shlroland/Custom_OpenClash_Rules/refs/heads/main/my-overwrite/Custom_Overwrite_ConfigURL.conf
```

Multi-provider overwrite module:

```text
https://raw.githubusercontent.com/shlroland/Custom_OpenClash_Rules/refs/heads/main/my-overwrite/Custom_Overwrite_Multi.conf
```

iStoreOS current-settings overwrite module:

```text
https://raw.githubusercontent.com/shlroland/Custom_OpenClash_Rules/refs/heads/main/my-overwrite/IStoreOS_Current_ConfigURL.conf
```

Final YAML downloaded by the overwrite module:

```text
https://raw.githubusercontent.com/shlroland/Custom_OpenClash_Rules/refs/heads/main/my-overwrite/Custom_Clash.yaml
```

## OpenClash Setup

1. Open OpenClash remote overwrite modules.
2. Add a new module:
   - Type: `http`
   - URL: use the overwrite module URL above.
3. Add environment variables.

For `Custom_Overwrite_ConfigURL.conf`:

   - `EN_KEY1`: final converted Clash/Mihomo YAML config URL

For `IStoreOS_Current_ConfigURL.conf`:

   - `EN_KEY1`: final converted Clash/Mihomo YAML config URL

For `Custom_Overwrite_Multi.conf`:

   - `EN_KEY1`: converted subscription URL 1
   - `EN_KEY2`: converted subscription URL 2
   - `EN_KEY3`: converted subscription URL 3
   - `EN_KEY4`: converted subscription URL 4
4. Save and apply.

## Notes

- `Custom_Overwrite_ConfigURL.conf` is the best fit when your URL already contains subscription conversion and can aggregate multiple upstream subscriptions.
- `IStoreOS_Current_ConfigURL.conf` preserves the inspected router posture: Meta core, `fake-ip`, IPv6 disabled, China IP bypass enabled, custom DNS enabled, sniffer enabled, and Smart disabled.
- `EN_KEY1` is required.
- This YAML declares four providers. For fewer subscriptions, prefer aggregating them with Sub-Store into one converted subscription URL and put it in `EN_KEY1`.
- The overwrite module controls OpenClash runtime settings such as Fake-IP, IPv6, China IP bypass, sniffer, Geo database update URLs, and selected config file path.
- The `.ini` files in `my-cfg/` are subscription conversion templates. They are not directly executable as OpenClash remote overwrite configs.
