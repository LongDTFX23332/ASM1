import pandas as pd
import numpy as np


def read_file(file_name):
    file_path = f'data/{file_name}.txt'
    valid_line = []
    invalid_line = []
    with open(file_path, 'r') as file:
        print(f'Successfully opened {file_name}.txt')
        print('**** ANALYZING ****')
        # Duyet qua tung dong cua file de kiem tra xem du lieu co thoa dieu kien
        for index, line in enumerate(file, start=0):
            # Loai bo ki tu xuong dong va phan tach cac cot du lieu bang dau ','
            line_list = line.strip('\n').split(',')
            student_id = line_list[0]
            answers = line_list[1:]
            if validate_data(student_id, answers, line):
                valid_line.append(line_list)
            else:
                invalid_line.append(line_list)

        if len(invalid_line) == 0:
            print('No errors found!')

        show_report(len(valid_line), len(invalid_line))
        # Chuyen doi cac du lieu hop le thanh data frame
        df = pd.DataFrame(valid_line)
        # Tao mot list column names [StudentID, Q1, Q2,....,Q25]
        columns_name = ['StudentID'] + [f'Q{i}' for i in range(1, 26)]
        df.columns = columns_name
        return df
    # try:

    #     except FileNotFoundError:
    #         print('File cannot be found.')


def show_report(valid_line_num, invalid_line_num):
    print('**** REPORT ****')
    print(f'Total valid lines of data: {valid_line_num}')
    print(f'Total invalid lines of data: {invalid_line_num}')


def validate_data(student_id: str, answers, line_str):
    # Kiem tra tinh hop le so cau tra loi phai la 25
    if len(answers) != 25:
        print('Invalid line of data: does not contain exactly 26 values:')
        print(line_str)
        return False
    # Kiem tra tinh hop le mssv bat dau bang chu N va 8 chu so
    if not student_id.startswith('N') or not student_id[1:].isdigit() or len(student_id) != 9:
        print('Invalid line of data: N# is invalid:')
        print(line_str)
        return False

    return True


def caculate_grade(student_answers):
    answer_key = "B,A,D,D,C,B,D,A,C,C,D,B,A,B,A,C,B,D,A,C,A,A,B,D,D".split(',')
    # Diem tra loi dung +4, khong tra loi 0, sai -1
    score = np.sum(
        np.where(student_answers == answer_key, 4,
                 np.where(student_answers == '', 0,
                          -1
                          )))
    return score


def report_score(scores):
    # thong ke diem
    average_score = np.mean(scores)
    min_score = np.min(scores)
    max_score = np.max(scores)
    range_score = max_score - min_score
    median_score = np.median(scores)

    print(f'Mean (average) score: {average_score}')
    print(f'Highest score: {max_score}')
    print(f'Lowest score: {min_score}')
    print(f'Range of scores: {range_score}')
    print(f'Median score: {median_score}')


def export_file():
    pass


def main():
    while True:
        try:
            file_name = input(
                'Enter a class file to grade (i.e. class1 for class1.txt): ')
            df = read_file(file_name)
            break
        except FileNotFoundError:
            print('File not found')
    # Tinh diem va them cot 'Scores' vao data frame
    df['Scores'] = df.apply(
        lambda row: caculate_grade(row[1:].to_numpy()), axis=1)

    report_score(df['Scores'].to_numpy())
    # Tao mot data frame moi co 2 cot StudentID va Scores de export ra file moi
    file_export_name = f'{file_name}_grades.txt'
    file_export_path = f'data/{file_export_name}'
    df_export = df[['StudentID', 'Scores']]
    df_export.to_csv(file_export_path, sep=',', index=False, header=False)
    print(f'# this is what {file_export_name} should look like')
    with open(file_export_path,'r') as file:
        for index, line in enumerate(file,start=0):
            print(line)


if __name__ == '__main__':
    main()
