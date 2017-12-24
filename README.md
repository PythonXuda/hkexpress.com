# hkexpress.com
# just a Air ticket spider
from selenium import webdriver
from lxml import etree
import time
#创建浏览器对象
driver = webdriver.PhantomJS()
# 发送请求
driver.get('https://www.hkexpress.com/en-hk')

# 等待加载
driver.implicitly_wait(10)

# 点击单程
driver.find_element_by_id('msingle').click()

# 点击输入框
driver.find_element_by_id('mOriginFrom').click()

# 点击地点
driver.find_element_by_xpath('//div[@id="mairportFrom"]//a[@id="from"][1]').click()

# 点击目的地
driver.find_element_by_xpath('//div[@id="mairportTo"]//a[@id="to"][1]').click()

# 点击日期输入框触发隐藏
driver.find_element_by_id('mDepartureDate').click()

# 选择日期
driver.find_element_by_xpath('//a[@class="ui-state-default"][1]').click()

# 点击搜索
driver.find_element_by_class_name('searchFlights').click()

# 等待跳转
time.sleep(3)
# 确认请求是否正确
# driver.save_screenshot('hk.png')

response = driver.page_source
html = etree.HTML(response)
num = html.xpath('//span[@class="totalPackage"]/text()')
currency = html.xpath('//span[@class="currency"]/text()')
# 价格
price = num + currency
# 出发地
f_addr= html.xpath('//h2[@class="title_t2"]/text()')

# 出发日期
date = html.xpath('//p[@class="yoursearch_box_departing"]/text()')

print(price,f_addr,date)
# 退出
driver.quit() 
