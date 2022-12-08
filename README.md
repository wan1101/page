# page
页面自动化操作
import time
from selenium import webdriver
from selenium.common import exceptions
from selenium.webdriver.common.by import By

from image_code import iam_code
from image_yzm import Yzm_image


driver_path = (r'C:\Program Files\Google\Chrome\Application/chromedriver.exe')  # 驱动位置
prefs = {'profile.default_content_settings.popups': 0, 'download.default_directory': r'F:\mbk'}  # 设置下载文件存放路径，这里要写绝对路径
options = webdriver.ChromeOptions()
options.add_experimental_option('prefs', prefs)
options.add_experimental_option("detach", True) #  不自动关闭浏览器
# options.add_argument('headless')    # 浏览器隐式启动
driver = webdriver.Chrome(executable_path=driver_path, options=options)


def ele_click(xpath):
    driver.find_element(By.XPATH, xpath).click()

def ele_send(xpath,s_keys):
    driver.find_element(By.XPATH,xpath).send_keys(s_keys)


driver.get('http://aq.changyou.com/card/showBindStatus.do')
driver.find_element(By.XPATH,'//*[@id="downMsg"]').click() # 下载绑定
time.sleep(3)
driver.find_element(By.XPATH,'/html/body/div[5]/div/div/div[2]/div/a[2]').click() # 不，是密保卡
time.sleep(2)
driver.find_element(By.XPATH,'//*[@id="downMsg"]').click() # 下载绑定
driver.find_element(By.XPATH,'/html/body/div[2]/div[2]/div[1]/a[1]/span').click() # 下载
time.sleep(5)
driver.find_element(By.XPATH,'/html/body/div[2]/div[2]/div[2]/a[1]/span').click() # 已下载，立即绑定


# =================================================================================================
 # 截图验证码
a = driver.find_element(By.XPATH,'//*[@id="imageCodeId"]')
a.screenshot("F:\yzm\Yzm.png")

ele_send('//*[@id="cn"]','1030128525@game.sohu.com') # 畅游通行证
ele_send('//*[@id="pwd"]','445566') # 密码
zuobiao1 = driver.find_element(By.XPATH,'/html/body/div[2]/div/form/div[5]/span[1]').text
num1 = iam_code(zuobiao1)
zuobiao2 = driver.find_element(By.XPATH, '/html/body/div[2]/div/form/div[5]/span[2]').text
num2 = iam_code(zuobiao2)
zuobiao3 = driver.find_element(By.XPATH, '/html/body/div[2]/div/form/div[5]/span[3]').text
num3 = iam_code(zuobiao3)
# 矩形数字
ele_send('//*[@id="d1"]',num1)
ele_send('//*[@id="d2"]',num2)
ele_send('//*[@id="d3"]',num3)
# 验证码
yzm = Yzm_image()
ele_send('//*[@id="authCode"]',yzm)
time.sleep(10)
# 下一步
ele_click('/html/body/div[2]/div/form/div[8]/a[1]/span')

time.sleep(10)



try:
    target = driver.find_element(By.XPATH,'//*[@id="formMsg"]') #  页面提示语
    print("shi")
    driver.find_element(By.XPATH, '//*[@id="authCode"]').clear() # 清空输入框
    a = driver.find_element(By.XPATH, '//*[@id="imageCodeId"]')
    a.screenshot("F:\yzm\Yzm.png")# 截取元素并存储
    time.sleep(3)
    yzm = Yzm_image() #识别
    ele_send('//*[@id="authCode"]', yzm) # 从新输入验证码
    time.sleep(10)
    ele_click('/html/body/div[2]/div/form/div[8]/a[1]/span') # 再次点击下一步

except exceptions.NoSuchElementException:
    print("bu")



# ================================================================================================================
time.sleep(10)
ele_click('//*[@id="02"]')
time.sleep(2)
ele_send('//*[@id="answer"]','zqxwce')

driver.find_element(By.XPATH,'//*[@id="imageCodeId2"]').screenshot("F:\yzm\Yzm.png")
yzm = Yzm_image()
ele_send('//*[@id="imageCodeId2"]',yzm)
time.sleep(5)
ele_click('//*[@id="questionForm"]/div[4]/a[1]/span')

