from selenium import webdriver
from selenium.webdriver.common.keys import Keys
from time import sleep as sleep
import time
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from selenium.webdriver.common.by import By
from selenium.webdriver.common.desired_capabilities import DesiredCapabilities
import pprint
import selenium

class schedule():
    def __init__(self, class_number, section, meeting_times, room, active_dates, prof, NBR, NBR_NUMBER, open):
        self.section = section
        self.open = open
        self.profs = prof
        self.class_number = class_number
        self.prof_rating = 0
        self.meeting_times = meeting_times
        self.room = room
        self.dates = active_dates
        self.NBR = NBR
        self.NBR_NUMBER = NBR_NUMBER

        # self.parse_meeting_times()
        # self.parse_prof()
        self.score = []
    def set_score(self, score):
        self.score.append(score)

    def parse_meeting_times(self):
        # parse things such as MoWe in here along with the times
        # use 24 hour clock for the times
        pass
    def parse_prof(self):
        # find the list of professors teaching the class
        pass


class rmp_parser ():
    def __init__(self):
        caps = DesiredCapabilities().CHROME
        caps["pageLoadStrategy"] = "none"
        self.driver = webdriver.Chrome(desired_capabilities=caps, executable_path=r'D:\Python\chromedriver.exe')


    def search_professors(self, name):
        # self.driver.desired_capabilities['eager']
        self.clear = False
        print('searching for', name)

        url = 'https://www.ratemyprofessors.com/'
        self.driver.get(url)

        print('got webpage')
        sleep(5)

        self.clear_popup()

        try:
            cookeies = self.driver.find_element_by_xpath('//*[@id="cookie_notice"]/a[1]')
            cookeies.click()
            print('cookies warning was successfully cleared')
        except:
            print('!cookies not removed')
            pass

        self.clear_popup()

        search = WebDriverWait(self.driver, 10).until(EC.presence_of_element_located((By.XPATH, '//*[@id="findProfessorOption"]')))
        # search = self.driver.find_element_by_xpath('//*[@id="findProfessorOption"]')
        search.click()
        print('starting search')

        self.clear_popup()

        school = WebDriverWait(self.driver, 10).until(EC.presence_of_element_located((By.XPATH, '//*[@id="searchProfessorSchool2"]')))
        # school = self.driver.find_element_by_xpath('//*[@id="searchProfessorSchool2"]')
        school.click()
        school.send_keys('university of nevada reno')

        self.clear_popup()

        prof = WebDriverWait(self.driver, 10).until(EC.presence_of_element_located((By.XPATH, '//*[@id="searchProfessorName"]')))
        # prof = self.driver.find_element_by_xpath('//*[@id="searchProfessorName"]')
        prof.click()
        prof.send_keys(name)

        self.clear_popup()

        activate_search= WebDriverWait(self.driver, 10).until(EC.presence_of_element_located((By.XPATH, '//*[@id="prof-name-btn"]')))
        # activate_search = self.driver.find_element_by_xpath('//*[@id="prof-name-btn"]')
        activate_search.click()
        print('starting search')

        self.clear_popup()


        result = WebDriverWait(self.driver, 10).until(EC.presence_of_element_located((By.XPATH, '//*[@id="searchResultsBox"]/div[2]/ul/li/a')))
        # result = self.driver.find_element_by_xpath('//*[@id="searchResultsBox"]/div[2]/ul/li/a')
        result.click()
        print('clicking result')

        self.clear_popup()

        score = WebDriverWait(self.driver, 10).until(EC.presence_of_element_located((By.XPATH, '//*[@id="mainContent"]/div[1]/div[3]/div[1]/div/div[1]/div/div/div'))).text
        print('score is %s for %s' % (score, name))

        return score

    def clear_popup(self):
        if self.clear == False:
            try:
                self.driver.switch_to.frame(self.driver.find_element_by_name('spout-unit-iframe'))
                popup = self.driver.find_element_by_xpath('//*[@id="spout-header-close"]')
                popup.click()
                print('popup was removed')
                self.clear = True
            except:
                # print('there was no pop up ? ')
                pass
            finally:
                self.driver.switch_to.default_content()

class Parser():
    def __init__ (self,driver_type='chrome'):
        if driver_type.lower() == 'chrome':
            self.driver = webdriver.Chrome()
        elif driver_type.lower()  == 'firefox':
            self.driver = webdriver.Firefox()
        self.base_search()

        self.rmp = rmp_parser()
    def base_search(self):
        url = r'https://css.nevada.unr.edu/psp/rncssprd/EMPLOYEE/HRMS/c/COMMUNITY_ACCESS.CLASS_SEARCH.GBL?FolderPath=PORTAL_ROOT_OBJECT.CO_EMPLOYEE_SELF_SERVICE.HC_CLASS_SEARCH_GBL'
        self.driver.get(url)
        sleep(1)
        print('allowing all days to be used')
        self.driver.switch_to.frame(self.driver.find_element_by_name('TargetContent'))
        days_to_include = self.driver.find_element_by_xpath('//*[@id="SSR_CLSRCH_WRK_INCLUDE_CLASS_DAYS$6"]')
        days_to_include.click()
        days_to_include.send_keys(Keys.ARROW_UP)
        days_to_include.send_keys(Keys.ENTER)

        # sets to undergraduate
        print('using undergrad classes only')
        class_type = self.driver.find_element_by_xpath('//*[@id="SSR_CLSRCH_WRK_ACAD_CAREER$2"]')
        class_type.click()
        for i in range(3):
            class_type.send_keys(Keys.ARROW_DOWN)
        class_type.send_keys(Keys.ENTER)

        # remove req for open classes
        print('searching for classes even if they are closed')
        open_only = self.driver.find_element_by_xpath('//*[@id="SSR_CLSRCH_WRK_SSR_OPEN_ONLY$3"]')
        open_only.click()

        # sets mode of instruction to in person
        # print('in person classes only')
        # instruct = self.driver.find_element_by_xpath('//*[@id="SSR_CLSRCH_WRK_INSTRUCTION_MODE$4"]')
        # instruct.click()
        # for i in range(5):
        #     instruct.send_keys(Keys.ARROW_DOWN)
        # instruct.send_keys(Keys.ENTER)

        # select mon / tues / wed / etc as valid class days
        print('selecting all the days of the week')
        for i in ['MON', 'TUES', 'WED', 'THURS', 'FRI', 'SAT', 'SUN']:
            name = 'SSR_CLSRCH_WRK_' + i + '$6'
            day = self.driver.find_element_by_id(name)
            day.click()

    def find_classes(self,NBR, NBR_NUMBER):
        subj = self.driver.find_element_by_xpath('//*[@id="SSR_CLSRCH_WRK_SUBJECT$0"]')
        subj.click()
        subj.send_keys(NBR)

        course_number = self.driver.find_element_by_xpath('//*[@id="SSR_CLSRCH_WRK_CATALOG_NBR$1"]')
        course_number.click()
        course_number.send_keys(NBR_NUMBER)
        sleep(4)

        b = self.driver.find_element_by_tag_name('body')
        b.click()

        print('searching')
        search = self.driver.find_element_by_xpath('//*[@id="CLASS_SRCH_WRK2_SSR_PB_CLASS_SRCH"]')
        search.click()

        try:
            print('checking to see if there are more than 50 classes')
            over_50_classes = WebDriverWait(self.driver, 2).until(EC.presence_of_element_located((By.XPATH, '//*[@id="#ICSave"]')))
            over_50_classes.click()
            print('there are more than 50 classes returned in your search')
        except selenium.common.exceptions.TimeoutException:
            print('there are x < 50 results')

        WebDriverWait(self.driver, 60*5).until(EC.presence_of_element_located((By.XPATH, r'//*[@id="MTG_CLASS_NBR$0"]')))

        #collect all of the information on the classes:
        print('now going to cycle through class data')

        deep_data = []
        profs = {}

        for i in range(10):
            try:
                i = str(i)
                print(i)
                class_number = r'//*[@id="MTG_CLASS_NBR$' + i + r'"]'
                class_name = r'//*[@id="MTG_CLASSNAME$' + i + r'"]'
                meeting_times = r'//*[@id="MTG_DAYTIME$' + i + r'"]'
                room = r'//*[@id="MTG_ROOM$' + i + r'"]'
                instructor = r'//*[@id="MTG_INSTR$' + i + r'"]'
                dates = r'//*[@id="MTG_TOPIC$' + i + r'"]'
                open = r'//*[@id="win0divDERIVED_CLSRCH_SSR_STATUS_LONG$' + i + r'"]/div/img'

                class_number_value = self.driver.find_element_by_xpath(class_number).text
                class_name_value = self.driver.find_element_by_xpath(class_name).text.split('\n')[0]
                meeting_times_value = self.driver.find_element_by_xpath(meeting_times).text
                room_value = self.driver.find_element_by_xpath(room).text.split('\n')
                dates_value = self.driver.find_element_by_xpath(dates).text

                prof = self.driver.find_element_by_xpath(instructor).text.split('\n')
                if type(prof) != list:
                    if prof not in profs.keys():
                        profs[prof] = self.rmp.search_professors(prof)
                    prof = [prof]

                else:
                    while True:
                        if 'Staff' in prof:
                            prof.remove('Staff')
                        else:
                            break
                    prof = list(set(prof))
                    for p in prof:
                        if p not in profs.keys():
                            profs[p] = self.rmp.search_professors(p)

                open_val = self.driver.find_element_by_xpath(open).get_attribute('alt')

                class_object = schedule(class_number_value, class_name_value, meeting_times_value, room_value, dates_value,prof, NBR, NBR_NUMBER, open_val)
                for p in prof:
                    class_object.set_score(profs[p])

                deep_data.append(class_object)
            except Exception as e:
                print(e)
                break
        print('all data from class search')

        for i in deep_data:
            pprint.pprint(i.__dict__)
        return deep_data

if __name__ == '__main__':
    start =time.time()
    parse = Parser('chrome')
    # parse.find_classes(NBR='MATH', NBR_NUMBER='285')
    classes = [['math', '310'],
              ['engr', '241'],
              ['ME','203'],
              ['phys', '181'],
              ['mse', '250']]
    data = []
    for row in classes:
        nbr, num = row[0], row[1]
        data.append(parse.find_classes(nbr,num))
        parse.base_search()

    print('type of data: ', type(data))
    print ('type of obj: ', type(data[0]))

    for subject in data:
        for i in subject:
            pprint.pprint(i.__dict__)
    print('process took %s' % (time.time() - start))
    # rmp = rmp_parser()
    # rmp.search_professors('Mark Pinsky')
