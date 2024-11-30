## add thư viện
const puppeteer = require("puppeteer-extra");
const StealthPlugin = require("puppeteer-extra-plugin-stealth");
const AnonymizeUAPlugin = require("puppeteer-extra-plugin-anonymize-ua");
const AdblockerPlugin = require("puppeteer-extra-plugin-adblocker");
const proxyChain = require('proxy-chain');
const fs = require('fs');
const path = require('path');

puppeteer.use(StealthPlugin());
puppeteer.use(AnonymizeUAPlugin());
puppeteer.use(
    AdblockerPlugin({
        blockTrackers: true,
    })
)

## download
npx @puppeteer/browsers install chrome@116.0.5793.0

## tạo folder chứa profile chromium: E:\puppeteer-auto-meta-proxy\chrome-profile
### Lệnh PowerShell
& "E:\puppeteer-auto-meta-proxy\chrome\win64-116.0.5793.0\chrome-win64\chrome.exe" --user-data-dir="E:\puppeteer-auto-meta-proxy\profile\Default"


### userDataDir không được trùng 

## puppeteer.launch
        launchBrowserWithProfile: async function (profilePath) {
            const userDataDir = `E:/puppeteer-auto-meta-proxy/chrome-profile-${profilePath}`;

            // Tạo thư mục userDataDir nếu chưa tồn tại
            if (!fs.existsSync(userDataDir)) {
                fs.mkdirSync(userDataDir, { recursive: true });
            }
            try {
                const browser = await puppeteer.launch({
                    headless: false,
                    ignoreDefaultArgs: ['--disable-extensions'],
                    executablePath: "E:\\puppeteer-auto-meta-proxy\\chrome\\win64-116.0.5793.0\\chrome-win64\\chrome.exe",
                    userDataDir: userDataDir,
                    args: [
                        `--profile-directory=${profilePath}`,
                        '--no-sandbox',
                        '--disable-setuid-sandbox'
                    ],
                    defaultViewport: null,

                });
                console.log('Browser launched successfully');
                return browser;
            } catch (error) {
                console.error('Error launching browser:', error);
                throw error;
            }
        }








