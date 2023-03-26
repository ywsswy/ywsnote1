void
Xapian::Document::Internal::set_data(const string &data_)
{
    data = data_;
    data_here = true;
}