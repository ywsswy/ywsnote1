def prn_obj(obj):
    print('\n'.join(['%s:%s' % item for item in obj.__dict__.items()]))