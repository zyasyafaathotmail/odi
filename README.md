# OneDrive-Index (One-Click Deploy with Vercel)

English | [中文简体](./README.zh-CN.md)

This project is a fork from [spencerwooo/onedrive-vercel-index](https://github.com/spencerwooo/onedrive-vercel-index), based on the archived version from the original author dated June 24, 2023. It includes some minor modifications that allow you to deploy it on Vercel for free, showcasing, sharing, previewing, and downloading your OneDrive files on a webpage. For specific deployment methods, please refer to the instructions below.

> This version has only been tested with an E5 Developer account. Other types of OneDrive accounts need further testing.

## Modifications

- In this version, some variables that needed to be set in the `api.config.js` and `site.config.js` configuration files in the `config/` are now set in the environment variables of Vercel. In this way, there is no need to - first fork the original repository - then manually modify the configuration file - and then deploy. Instead, you can directly click the one-click deployment button in this document, enter the values of the environment variables during the deployment process, and then complete the deployment.

- Additionally, this version is set to automatically close the OAuth authentication channel after OAuth authentication is completed. This is to prevent malicious individuals from easily obtaining user configuration information through the OAuth authentication URL link.

## Demo

The [Demo](https://odi-demo.freeloop.one) of this One-Click Deploy version. | The [Demo](https://drive.swo.moe) (UNMAINTAINED) by the original author.

![demo](./public/demo.png)

## Getting Started

### Preparations

1. **Setting up the API permissions for your OneDrive account.**

- This project retrieves the file list and download links by calling OneDrive's API, so setting up the API permissions for your OneDrive account is essential. Please refer to the [DOCS](https://ovi.swo.moe/docs/advanced#modify-api-permissions).

> The three API permissions that need to be set up are: `user.read`, `files.read.all`, `offline_access`.

2. **Prepare the five [necessary environmental variables (click to view)](#necessary-variables) to be filled in during deployment on Vercel.**

### Deploying to Vercel

3. **Once you're prepared, you can click the button below to deploy:**

[![Deploy with Vercel](https://vercel.com/button)](https://vercel.com/new/git/clone?repository-url=https%3A%2F%2Fgithub.com%2FiRedScarf%2Fonedrive-index&env=NEXT_PUBLIC_SITE_TITLE,USER_PRINCIPAL_NAME,BASE_DIRECTORY,CLIENT_ID,CLIENT_SECRET)

> - If you have folders that need password protection.
>
> [![Deploy with Vercel](https://vercel.com/button)](https://vercel.com/new/git/clone?repository-url=https%3A%2F%2Fgithub.com%2FiRedScarf%2Fonedrive-index&env=NEXT_PUBLIC_SITE_TITLE,USER_PRINCIPAL_NAME,BASE_DIRECTORY,NEXT_PUBLIC_PROTECTED_ROUTES,CLIENT_ID,CLIENT_SECRET) with `NEXT_PUBLIC_PROTECTED_ROUTES`
>
> - If you have multiple OneDrive accounts that need to use the same Redis database.
>
> [![Deploy with Vercel](https://vercel.com/button)](https://vercel.com/new/git/clone?repository-url=https%3A%2F%2Fgithub.com%2FiRedScarf%2Fonedrive-index&env=NEXT_PUBLIC_SITE_TITLE,USER_PRINCIPAL_NAME,BASE_DIRECTORY,CLIENT_ID,CLIENT_SECRET,KV_PREFIX) with `KV_PREFIX`
>
> - If you need to deploy multiple OneDrive-Index, and all have folders that need password protection.
>
> [![Deploy with Vercel](https://vercel.com/button)](https://vercel.com/new/git/clone?repository-url=https%3A%2F%2Fgithub.com%2FiRedScarf%2Fonedrive-index&env=NEXT_PUBLIC_SITE_TITLE,USER_PRINCIPAL_NAME,BASE_DIRECTORY,NEXT_PUBLIC_PROTECTED_ROUTES,CLIENT_ID,CLIENT_SECRET,KV_PREFIX) with `NEXT_PUBLIC_PROTECTED_ROUTES` & `KV_PREFIX`

4. **After the initial successful deployment, the deployed page will return a 404 error because we still need to connect to the Redis database.**

> `REDIS_URL`:If you are encountering Redis database for the first time, I strongly recommend using Upstash, which is free and deeply integrated with Vercel. For details, refer to [Vercel Integration](https://docs.upstash.com/redis/howto/vercelintegration). Follow the instructions to set it up in Vercel's [Upstash Integration](https://vercel.com/integrations/upstash)(simply create a new database in the `Redis` of Upstash, then create a new integration in `Vercel Integrations`, and associate the just deployed OneDrive-Index project with the Redis database), it will automatically fill in the environment variables after project deployment.

5. **After `REDIS_URL` is successfully set, redeploy the project again.**

6. **After successful deployment, when you visit your `onedrive-vercel-index` page for the first time, it will guide you to perform OAuth authentication (quite simple). For details, please refer to the [Instructions](https://ovi.swo.moe/zh/docs/getting-started#authentication) written by the original author.**

## Environment Variables

### Necessary Variables
| Name | Description | Original Path | Note |
| --- | --- | --- | --- |
| `NEXT_PUBLIC_SITE_TITLE` | Title of the display page | `config/site.config.js` | e.g. Nicaragua's richest man's OneDrive |
| `USER_PRINCIPAL_NAME` | Your OneDrive account | `config/site.config.js` | **Case-sensitive** |
| `BASE_DIRECTORY` | The OneDrive directory you want to share | `config/site.config.js` | `/directory name`, root directory is `/` |
| `CLIENT_ID` | The client ID of the app you registered in Microsoft Azure | `config/api.config.js` | The one provided by the original author has expired, it is recommended to register one yourself, the validity period can be set to two years (anyway, you have to set the API permissions of the account, by the way). The acquisition method refers to the [DOCS](https://ovi.swo.moe/docs/advanced#using-your-own-clientid-and-clientsecret) |
| `CLIENT_SECRET` | The client secret of the app registered in Microsoft Azure | `config/api.config.js` | The acquisition method is the same, especially note that this **needs to encrypt the original secret with AES** (can be done in the [DOCS](https://ovi.swo.moe/docs/advanced#modify-configs-in-apiconfigjs)) |

### Optional Variables
| Name | Description | Original Path | Note |
| --- | --- | --- | --- |
| `NEXT_PUBLIC_PROTECTED_ROUTES` | The path of the folder that needs password access | `config/site.config.js` | Format: `/route1,/route2`, multiple paths are separated by `,` |
| `NEXT_PUBLIC_EMAIL` | Contact Email displayed in the upper right corner | `config/site.config.js` | `example@example.com` |
| `KV_PREFIX` | Prefix for KV storage (key-value pair storage) | `config/site.config.js` | Upstash only provides a free `Redis` database, if you want to deploy multiple OneDrive-Index, you can set different `KV_PREFIX` values for different Index, so there will be no key value conflict |

## Documentation

**For more usage methods, please refer to the [DOCS](https://ovi.swo.moe/docs/getting-started) written by the original author.**

## Security Risks

- In the archived version of the original author, the `userPrincipalName`, `clientId`, and `obfuscatedClientSecret` of the OneDrive account of the deployer are exposed in the source code of the web page.

> This version checks whether authentication has already been passed when performing the OAuth authentication process. If it has, it redirects to the homepage, otherwise, it proceeds with the OAuth authentication process. It attempts to prevent individuals with malicious intent from obtaining the values of `clientId` and `obfuscatedClientSecret` through the link address of OAuth authentication.

- Because of the design decision of Next.js, environment variables starting with `NEXT_PUBLIC_` are not only available on the server side, but also on the client side (browser). This means that any environment variable starting with `NEXT_PUBLIC_` will be included in the built JavaScript file and will be sent to the user's browser. Therefore, anyone visiting your website can view the values of these environment variables by viewing the source code of the website or network requests.

> This version uses non-`NEXT_PUBLIC_` prefixed environment variables for the `userPrincipalName`, `clientId`, `obfuscatedClientSecret`, and `baseDirectory` variables, making it as difficult as possible for website visitors to easily obtain your OneDrive account, ClientID, and ClientSecret information.

## Todo List

- Put the password in the environment variables instead of the `.password` file.

> However, in this way, it is more difficult to set different access passwords for different encrypted directories.

- Redesign the LOGO. The contrast of the original LOGO is too low, and it is not consistent enough with the style of other icons and fonts on the page.

## License

[MIT License](LICENSE)

© 2021-2023 [spencer woo](https://spencerwoo.com)

© 2023 [iRedScarf](https://github.com/iRedScarf)

<div align="center">
    Made by <a href="https://spencerwoo.com">spencer woo</a> | Modified by <a href="https://github.com/iRedScarf">iRedScarf
</div>
