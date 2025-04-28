// ==UserScript==
// @name         模拟请求并获取文章ID和打开链接
// @namespace    http://tampermonkey.net/
// @version      1.00
// @description  通过POST请求获取文章ID数组，并根据ID打开链接
// @author       You
// @match        https://lawplatform.chinaunicom.cn/web/*
// @grant        GM_xmlhttpRequest
// ==/UserScript==

(function() {
    'use strict';

    var globalToken = 'null';
    let clickedText = "1";
    let mmPayload = "ellDOxF5A+qP5oKsUrzTgNl1iAKpPLBN0RFJtFlraafIhE6y7CoyIr2H+zMwIuaVuTphjaJz3lmD/ykMRaVk2zuzdOzJyyTjzb+hOzM6N9e1qhtTXaCz8zBl9EyGuXqBMYIWikabS5tOa1wsDJgYSQHB53wtHD6TOQQbA3A52GKPvw11arG4KmbbhbL55vS8OPMp1V2DPLDHwnDrZbOWEyVHzSCeoxF0GafEsx/Lg7naU631N2TpZcp5Iyjek6xlue2RDsgW1qKVxh4AktT8UAJgeKreHF8aA6L3Je37n521yIfvLov+bVnHkvvYtX/apkurlC31a9r5QmKK3L6gLTlSRFMRWIcSEiT2Z7csqGiytFokQETgE2vQiJB93Y7qX4rqk0kpA6Z2OpelPHSwM9bqBxw5V2EPvd+fwJraYH8=";
    // POST 请求函数，用于获取文章 ID 数组
    function postRequest(pageNum,tokenFromCookies,mmPayload) {
        return new Promise((resolve, reject) => {
            //console.log('从 cookies 获取的令牌:', globalToken);
            const url = `https://lawplatform.chinaunicom.cn/api/legal/more/article/list?pageSize=10&pageNum=${pageNum}`;
            const headers = {
                "Accept": "application/json, text/plain, */*",
                "Accept-Encoding": "gzip, deflate, br, zstd",
                "Accept-Language": "zh-CN,zh;q=0.9,en-US;q=0.8,en;q=0.7",
                "Authorization": `Bearer ${globalToken}`,
                "Connection": "keep-alive",
                //"Content-Length": "457",
                "Content-Type": "application/json",
                "Cookie": `noticeNum=0; UC_SSO_SESSIONID_WEB=CD74CC11E8CF44A1B5B6E87218CCE4BA; token=${globalToken}`,
                "Host": "lawplatform.chinaunicom.cn",
                "Origin": "https://lawplatform.chinaunicom.cn",
                "Referer": "https://lawplatform.chinaunicom.cn/web/",
                "Sec-Fetch-Dest": "empty",
                "Sec-Fetch-Mode": "cors",
                "Sec-Fetch-Site": "same-origin",
                "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/135.0.0.0 Safari/537.36",
                "devVersion": "qdcs",
                //"request-id": "2fd14fbcaa284d149314655461e67844",
                "sec-ch-ua": "\"Google Chrome\";v=\"135\", \"Not-A.Brand\";v=\"8\", \"Chromium\";v=\"135\"",
                "sec-ch-ua-mobile": "?0",
                "sec-ch-ua-platform": "\"Windows\""
            };
            //console.log('参数1:', headers);
            // 请求的body

            /*const requestBody = {
                "mm":"ellDOxF5A+qP5oKsUrzTgNl1iAKpPLBN0RFJtFlraafIhE6y7CoyIr2H+zMwIuaVuTphjaJz3lmD/ykMRaVk2zuzdOzJyyTjzb+hOzM6N9e1qhtTXaCz8zBl9EyGuXqBMYIWikabS5tOa1wsDJgYSQHB53wtHD6TOQQbA3A52GKPvw11arG4KmbbhbL55vS8OPMp1V2DPLDHwnDrZbOWEyVHzSCeoxF0GafEsx/Lg7naU631N2TpZcp5Iyjek6xlue2RDsgW1qKVxh4AktT8UAJgeKreHF8aA6L3Je37n521yIfvLov+bVnHkvvYtX/apkurlC31a9r5QmKK3L6gLTlSRFMRWIcSEiT2Z7csqGiytFokQETgE2vQiJB93Y7qX4rqk0kpA6Z2OpelPHSwM9bqBxw5V2EPvd+fwJraYH8="
            };//普法专刊，这里需要有个载荷的字典，无法从页面获得
            console.log('参数1:', requestBody);*/

            const requestBody = {
                "mm": `${mmPayload}`
            };//普法专刊，这里需要有个载荷的字典，无法从页面获得
            console.log('参数1:', requestBody);

            // 发起POST请求
            GM_xmlhttpRequest({
                method: "POST",
                url: url,
                headers: headers,
                data: JSON.stringify(requestBody),
                onload: function(response) {
                    try {
                        // 输出响应内容
                        //console.log('被执行一次');
                        const data = JSON.parse(response.responseText);
                        console.log('data:',data);
                        const articleIds = data.rows.map(item => item.articleId);
                        resolve(articleIds); // 返回文章JSON
                    } catch (e) {
                        reject('解析JSON失败: ' + e.message);
                    }
                },
                onerror: function(error) {
                    reject('请求失败');
                }
            });
        });
    }

    // 打开链接函数，根据文章ID数组打开链接
    function openLinks(articleIds) {
        articleIds.forEach(id => {
            const articleUrl = `https://lawplatform.chinaunicom.cn/web/#/publicity/operationsManagement/review/index?type=3&articleId=${id}`;
            console.log("url:",articleUrl);
            window.open(articleUrl, '_blank'); // 打开新标签页
        })
    }
    function waitForElement(selector, timeout = 5000) {
        return new Promise((resolve, reject) => {
            const startTime = Date.now();

            const checkExist = () => {
                const element = document.querySelector(selector);
                if (element) {
                    resolve(element);
                } else if (Date.now() - startTime > timeout) {
                    reject(new Error("元素等待超时：" + selector));
                } else {
                    requestAnimationFrame(checkExist);
                }
            };

            checkExist();
        });
    }

    function like(){
        const currentURL = window.location.href;
        const regex = /^https:\/\/lawplatform\.chinaunicom\.cn\/web\/#\/publicity\/operationsManagement\/review\/index\?type=\d+&articleId=[^&]+$/;
        //https://lawplatform.chinaunicom.cn/web/#/publicity/operationsManagement/review/index?type=3&articleId=129ad8e3b1464aa8820e10e5acecb14f
        // 检查是否是指定的 URL
        if (regex.test(currentURL)) {
            // 只在指定页面 URL 执行的代码
            console.log("脚本在点赞页面执行");
            (async () => {
                try {
                    const selector = "#ant-layout-content > div.review-process-layout-content > div.review-process-layout-content-left > div > div.review-content-bottom";
                    const el = await waitForElement(selector);
                    setTimeout(function() {
                    el.click();
                    console.log("点击成功");
                    window.close();
                }, 5000); // 延迟 5 秒
                   
                } catch (err) {
                    console.error("出错啦：", err.message);
                }
            })();

        } else {
            console.log("当前页面不是指定的点赞页面，脚本不会执行");
        }

    }

    async function captureClick(event) {
        // 检查是否点击了 <a> 标签，并且该标签有 rel="nofollow" 属性
        if (event.target.tagName === 'A' && event.target.getAttribute('rel') === 'nofollow') {
            clickedText = event.target.textContent.trim(); // 获取点击元素中的文本
            console.log(`用户点击了数字：${clickedText}`);
        }
        else if (event.target.tagName === 'DIV' &&
                 event.target.classList.contains('publicity-more-left-cart-column-list-item') &&
                 event.target.textContent.trim() === '法律解读') {
            // 你可以在这里执行你的逻辑
            mmPayload = "ellDOxF5A+qP5oKsUrzTgNl1iAKpPLBN0RFJtFlraafIhE6y7CoyIr2H+zMwIuaVuTphjaJz3lmD/ykMRaVk2y5dtcEVVGi+3mbJ3aA/IGGHEQcMAOs2YzTSEAnde2ymkNIlXPD/MJX01pxbuyCAfgHB53wtHD6TOQQbA3A52GKPvw11arG4KmbbhbL55vS8OPMp1V2DPLDHwnDrZbOWEyVHzSCeoxF0GafEsx/Lg7naU631N2TpZcp5Iyjek6xlue2RDsgW1qKVxh4AktT8UAJgeKreHF8aA6L3Je37n521yIfvLov+bVnHkvvYtX/apkurlC31a9r5QmKK3L6gLTlSRFMRWIcSEiT2Z7csqGiytFokQETgE2vQiJB93Y7qX4rqk0kpA6Z2OpelPHSwM9bqBxw5V2EPvd+fwJraYH8=";
            console.log('点击了“法律解读”');
        }
        else if (event.target.tagName === 'DIV' &&
                 event.target.classList.contains('publicity-more-left-cart-column-list-item') &&
                 event.target.textContent.trim() === '沃学民法典') {
            // 你可以在这里执行你的逻辑
            mmPayload = "ellDOxF5A+qP5oKsUrzTgNl1iAKpPLBN0RFJtFlraafIhE6y7CoyIr2H+zMwIuaVuTphjaJz3lmD/ykMRaVk29IuRXMbbyPk3Xf5JLquXpgzL6bX/3ZToOZOzme+ozsjbWJDmv56W4gAr6OxWnQg3wHB53wtHD6TOQQbA3A52GKPvw11arG4KmbbhbL55vS8OPMp1V2DPLDHwnDrZbOWEyVHzSCeoxF0GafEsx/Lg7naU631N2TpZcp5Iyjek6xlue2RDsgW1qKVxh4AktT8UAJgeKreHF8aA6L3Je37n521yIfvLov+bVnHkvvYtX/apkurlC31a9r5QmKK3L6gLTlSRFMRWIcSEiT2Z7csqGiytFokQETgE2vQiJB93Y7qX4rqk0kpA6Z2OpelPHSwM9bqBxw5V2EPvd+fwJraYH8=";
            console.log('点击了“沃学民法典”');
        }
        else if (event.target.tagName === 'DIV' &&
                 event.target.classList.contains('publicity-more-left-cart-column-list-item') &&
                 event.target.textContent.trim() === '党内法规') {
            // 你可以在这里执行你的逻辑
            mmPayload = "ellDOxF5A+qP5oKsUrzTgNl1iAKpPLBN0RFJtFlraafIhE6y7CoyIr2H+zMwIuaVuTphjaJz3lmD/ykMRaVk2+mqXvQqP3mObLey8y7A2PBFwuHWT7wy6/h5ENDfqlJgr6AF/RKikaMlzKYeufOlRgHB53wtHD6TOQQbA3A52GKPvw11arG4KmbbhbL55vS8OPMp1V2DPLDHwnDrZbOWEyVHzSCeoxF0GafEsx/Lg7naU631N2TpZcp5Iyjek6xlue2RDsgW1qKVxh4AktT8UAJgeKreHF8aA6L3Je37n521yIfvLov+bVnHkvvYtX/apkurlC31a9r5QmKK3L6gLTlSRFMRWIcSEiT2Z7csqGiytFokQETgE2vQiJB93Y7qX4rqk0kpA6Z2OpelPHSwM9bqBxw5V2EPvd+fwJraYH8=";
            console.log('点击了“党内法规”');
        }
        else if (event.target.tagName === 'DIV' &&
                 event.target.classList.contains('publicity-more-left-cart-column-list-item') &&
                 event.target.textContent.trim() === '市场线合规') {
            // 你可以在这里执行你的逻辑
            mmPayload = "ellDOxF5A+qP5oKsUrzTgNl1iAKpPLBN0RFJtFlraafIhE6y7CoyIr2H+zMwIuaVuTphjaJz3lmD/ykMRaVk29IuRXMbbyPk3Xf5JLquXpgzL6bX/3ZToOZOzme+ozsjX2bHE9xp7lXAADhh36pT43YouNQnwZTwv28+MXv3zPcAjcjwPcHYtRrfwjEl6EVQRQmztp0MG8Qob7BfTZix/k+1+lNwraJoNasYSDHFeoVoVYNiUG63BZJQjYXu5SayAE2N7LE3Skqf/YOZzSP15kHVQMOD9dv1/AhXvMTEVcC4f2I9dp4paYKuWEGrgX9IgoaqUZiZJooFAQ4K4IkMCPHNwoUIc9mBgYOdjmoPpr7IXoApzKAEiPIBS5hdOFltxZTfjjwfz9uZ4AsEGyrD8RY+JHnG6j1A4CcsZnhLovA=";
            console.log('点击了“市场线合规”');
        }
        else if (event.target.tagName === 'DIV' &&
                 event.target.classList.contains('publicity-more-left-cart-column-list-item') &&
                 event.target.textContent.trim() === '网络线合规') {
            // 你可以在这里执行你的逻辑
            mmPayload = "ellDOxF5A+qP5oKsUrzTgNl1iAKpPLBN0RFJtFlraafIhE6y7CoyIr2H+zMwIuaVuTphjaJz3lmD/ykMRaVk29IuRXMbbyPk3Xf5JLquXpgzL6bX/3ZToOZOzme+ozsjX0grp6xYFLtaRUdiuZRlzHYouNQnwZTwv28+MXv3zPcAjcjwPcHYtRrfwjEl6EVQRQmztp0MG8Qob7BfTZix/k+1+lNwraJoNasYSDHFeoVoVYNiUG63BZJQjYXu5SayAE2N7LE3Skqf/YOZzSP15kHVQMOD9dv1/AhXvMTEVcC4f2I9dp4paYKuWEGrgX9IgoaqUZiZJooFAQ4K4IkMCPHNwoUIc9mBgYOdjmoPpr7IXoApzKAEiPIBS5hdOFltxZTfjjwfz9uZ4AsEGyrD8RY+JHnG6j1A4CcsZnhLovA=";
            console.log('点击了“网络线合规”');
        }
        else if (event.target.tagName === 'DIV' &&
                 event.target.classList.contains('publicity-more-left-cart-column-list-item') &&
                 event.target.textContent.trim() === '职能线合规') {
            // 你可以在这里执行你的逻辑
            mmPayload = "ellDOxF5A+qP5oKsUrzTgNl1iAKpPLBN0RFJtFlraafIhE6y7CoyIr2H+zMwIuaVuTphjaJz3lmD/ykMRaVk29IuRXMbbyPk3Xf5JLquXpgzL6bX/3ZToOZOzme+ozsjyQHxiUkUaFsg6WXDYH91j3YouNQnwZTwv28+MXv3zPcAjcjwPcHYtRrfwjEl6EVQRQmztp0MG8Qob7BfTZix/k+1+lNwraJoNasYSDHFeoVoVYNiUG63BZJQjYXu5SayAE2N7LE3Skqf/YOZzSP15kHVQMOD9dv1/AhXvMTEVcC4f2I9dp4paYKuWEGrgX9IgoaqUZiZJooFAQ4K4IkMCPHNwoUIc9mBgYOdjmoPpr7IXoApzKAEiPIBS5hdOFltxZTfjjwfz9uZ4AsEGyrD8RY+JHnG6j1A4CcsZnhLovA=";
            console.log('点击了“职能线合规”');
        }
        else if (event.target.tagName === 'DIV' &&
                 event.target.classList.contains('publicity-more-left-cart-column-list-item') &&
                 event.target.textContent.trim() === '涉外合规') {
            // 你可以在这里执行你的逻辑
            mmPayload = "ellDOxF5A+qP5oKsUrzTgNl1iAKpPLBN0RFJtFlraafIhE6y7CoyIr2H+zMwIuaVuTphjaJz3lmD/ykMRaVk2xaY9jIVBd6EKZJWip9GuNqTBhrRE5tSi56/xJzv1MdFr6AF/RKikaMlzKYeufOlRgHB53wtHD6TOQQbA3A52GKPvw11arG4KmbbhbL55vS8OPMp1V2DPLDHwnDrZbOWEyVHzSCeoxF0GafEsx/Lg7naU631N2TpZcp5Iyjek6xlue2RDsgW1qKVxh4AktT8UAJgeKreHF8aA6L3Je37n521yIfvLov+bVnHkvvYtX/apkurlC31a9r5QmKK3L6gLTlSRFMRWIcSEiT2Z7csqGiytFokQETgE2vQiJB93Y7qX4rqk0kpA6Z2OpelPHSwM9bqBxw5V2EPvd+fwJraYH8=";
            console.log('点击了“涉外合规”');
        }
        else if (event.target.tagName === 'DIV' &&
                 event.target.classList.contains('publicity-more-left-cart-column-list-item') &&
                 event.target.textContent.trim() === '生活有法') {
            // 你可以在这里执行你的逻辑
            mmPayload = "ellDOxF5A+qP5oKsUrzTgNl1iAKpPLBN0RFJtFlraafIhE6y7CoyIr2H+zMwIuaVuTphjaJz3lmD/ykMRaVk26K0QVFYtT0EYV5vIqzRgl4Mj/LmXSEzZc23HyRhoXcIpKXlDvecK8PuJjVua7l8H82rC1wRAPus4MIN9W6WhLYHGVkPn5rpwqLuC10LhQVuvNNbFsMHISZwtlQPql0FgrQVMHPm+GqJJSstlFG1nhkUvY9OPXqF13uOLdKfZK6m9lVvhpT9Stsc1lBAuleRxGk0B5uepf97oBezQ6rVXKRVLbf2krdC84MLaxDOoiCZjUczEEYBcSQ5AdwlZXjnZtrQKQqH5MmayD15p+59ZYKaimcLINAoHglqn0sJZg4Gck8MhRjIAc/w7M4EYSwmLvjIfAuL403vcEX5BiXD6Qo=";
            console.log('点击了“生活有法”');
        }
        else if (event.target.tagName === 'DIV' &&
                 event.target.classList.contains('publicity-more-left-cart-column-list-item') &&
                 event.target.textContent.trim() === '普法专刊') {
            // 你可以在这里执行你的逻辑
            mmPayload = "ellDOxF5A+qP5oKsUrzTgNl1iAKpPLBN0RFJtFlraafIhE6y7CoyIr2H+zMwIuaVuTphjaJz3lmD/ykMRaVk2zuzdOzJyyTjzb+hOzM6N9e1qhtTXaCz8zBl9EyGuXqBMYIWikabS5tOa1wsDJgYSQHB53wtHD6TOQQbA3A52GKPvw11arG4KmbbhbL55vS8OPMp1V2DPLDHwnDrZbOWEyVHzSCeoxF0GafEsx/Lg7naU631N2TpZcp5Iyjek6xlue2RDsgW1qKVxh4AktT8UAJgeKreHF8aA6L3Je37n521yIfvLov+bVnHkvvYtX/apkurlC31a9r5QmKK3L6gLTlSRFMRWIcSEiT2Z7csqGiytFokQETgE2vQiJB93Y7qX4rqk0kpA6Z2OpelPHSwM9bqBxw5V2EPvd+fwJraYH8=";
            console.log('点击了“普法专刊”');
        }
        if (clickedText && mmPayload) { //clickedText && mmPayload两个都有值再执行
            // 等待 postRequest 的返回结果
            const articleIds = await postRequest(clickedText,globalToken,mmPayload);
            //使用获取到的文章ID数组
            console.log("文章ID数组:", articleIds);
            openLinks(articleIds); // 打开文章链接
        }

    }

    // 从 cookies 获取令牌
    function getCookie(name) {
        const value = `; ${document.cookie}`;
        const parts = value.split(`; ${name}=`);
        if (parts.length === 2) return parts.pop().split(';').shift();
        return null;
    }



    // 主函数：调用POST请求获取文章ID并打开链接
    async function main() {
        like();
        globalToken = getCookie('token'); // 替换为实际的 cookie 名称
        //console.log(globalToken);  // 打印查看 token 是否成功
        // 获取当前页面的 URL
        const currentURL = window.location.href;
        const regex = /^https:\/\/lawplatform\.chinaunicom\.cn\/web\/#\/publicityPage\/more\/index.*/;
        // 检查是否是指定的 URL
        if (regex.test(currentURL)) {
            // 只在指定页面 URL 执行的代码
            console.log("脚本在指定页面执行");
            // 监听点击事件并调用函数
            document.addEventListener('click', captureClick);


        } else {
            console.log("当前页面不是指定的页面，脚本不会执行");
        }

    }



    // 执行主函数
    main();
})();
